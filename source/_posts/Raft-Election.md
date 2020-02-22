---
title: Raft选举
date: 2020-02-22 17:57:58
tags: [Raft]
---


这是关于Raft论文选举部分的整理


#### 5.1 Raft基础

raft集群包含多台机器，典型是5个，5个可以允许2台挂掉

任意时刻，一台机器只可能一种状态，状态可能是：leader，follower和candidate

正常操作情况下，只有一个leader，剩下的都是follower

follower是被动的，它们只回答leader和candidate的请求

leader处理所有客户端请求，如果有客户端联系了follower，follower会将客户端重定向给leader

candidate用于选举新leader

在Raft中，时间按照term分割，term长度不限，使用整数连续编号

每一个term的开始时一段选举期，选举期中会有1位或多位candidate竞选

如果一个candidate赢了一次选举，它就变成leader。有些情况下，可能出现分裂的投票（split vote，脑裂？），这时term结束，且无leader产生，进入下一轮term。Raft保证任意一个term内最多只会有1个leader（也可能是0个，未选举成功）

Term在Raft中扮演了逻辑锁角色，机器可以依据term来检查信息是否时过时的。每一个机器都存有当前term序号，序号单调递增

机器间通信时会交换各自的当前term值，小的那一方会将自己的当前term号改为和大的一致。如果小的那一方的身份是candidate或者leader，那么它将立即转入follower状态。如果一台机器收到一个带有旧term号的请求(request),它将拒绝这个请求

Raft机器间使用RPC通信，基本共识算法只需要两种RPC
- RequestVote   在选举期由candidate发起
- AppendEnties  由leader发起，用于复制log entry和提供heartbeat
另外还有一种RPC可以用于机器间转移快照

如果没有收到RPC响应，机器会重试。为了性能，RPC发起是并行的


#### 5.2 选举

Raft使用心跳机制来触发选举

机器启动时，状态都是follower

follower只要一直能收到来自leader或者candidate的RPC，它就保持follower状态

leader会定期给所有follower发heartbeat来保持权力（authority，合法性？）

heartbeat是AppendEnties类RPC，只是不包含log entry

follower如果在election timeout这段时间内没有收到任何消息，它就认为当前没有leader，当前应该处在选举期

开始选举时，follower将它的term加一，并转为candidate状态，通过RequestVote给集群中所有机器发RPC投票给自己

一个candidate的后续状态可能是下面3种：
- 赢了选举
	- 要求：必须收到集群中的大多数投票支持，term要保证在当期
	- 在一个term期间，一台机器只能投一个candidate
	- 投票人的投票原则是先来后到，谁先来投谁
	- 大多数的限制保证了一个term最多只能选出来一个leader
	- 一个candidate被选为leader后，它会给所有其它机器发送heartbeat维持权威，同时防止出现新选举
- 另一台机器赢了
	- candidate等票时，可能收到一个AppendEntries的RPC，并且RPC的发送方生成自己的leader
	- 如果那个leader的term号大于等于candidate自己的term号，那么candidate就承认那个leader的合法性，自己转为follower状态
	- 如果那个term号小于candidate的term号，那个RPC会被拒绝，candiate保持自己的candidate状态
- 一段时间后，没有选出来
	- 如果多个candidate同时出现，可能导致没有一个candidate能拿到大多数票
	- 发生这种情况时，每一个candidate都会超时，candidate将term加一然后发送新一轮的RequestVote， 进入下一轮选举
	- 这种选举分裂可能无限循环下去，故而需要借助于另外的方法来解决。Raft使用election timeout时间随机化来确保较少出现分裂情况，且当情况出现时，能很快地解决
	- Election timeout是从一个固定区间随机选取（例如150~300ms），这样可以把所有机器的超时时间分散开，这样大多数时候，第一个超时出现时，其他都还没有超时，然后这个最先超时的就会赢得选举
	- 随机化机制也用于选举分裂的情况，candidate在选举开始会重新随机化一个election timeout，直到这个election timeout超时才启动下一轮选举

> 算法设计者们开始时曾打算使用一个ranking系统，rank高的会被优先推选出来当leader，但它还会有一些问题，即使经过调整还是会有一些例外情况，最后结论是随机重试方法更直观，也更容易理解
