---
title: 6.824 Lab2B 小结
date: 2020-02-29 22:15:47
tags: [分布式, Raft, 6.824]
---

下午3点多终于把2B的测试全过了。

这几天一个一个地过，调试log全部混杂在一起，很不容易看。

2B部分的代码质量写得比较差，后面又有尾大不掉的问题，不敢擅动，怕影响之前的结果。

先前花了点时间把`TestFailNoAgree2B`不过的问题解决掉，具体问题限制也想不起来了。只写下今天完成的这俩。

#### TestBackup2B

这个问题的根源在于我对于heartbeat的处理。

因为AppendEntries在entries参数为空时，就是heartbeat。我原想heartbeat就不要参与日志写、状态写类的事情，这样可以把影响缩小到仅有有效负载时。但根据测试情况，如果heartbeat不参与处理，没能能使得最终达成aggrement，后来仔细想想，确实是需要的，heartbeat的作用有类似与TCP中的ACK作用，虽然信息不多，但对于双方通信确认几个参数是很重要的。

但是如果将heartbeat也纳入对log的调整和apply到状态机，会引入这样的问题：


```conf
# 参数类型 [cmd, Term]

0 [1, 1] [2, 1] ... [51, 1]
1 [1, 1] [2, 1] ... [51, 1]
2 [1, 1] [x, 7] ... [yy, 7] [52, 7] ... [101, 7]
3 [1, 1] [x, 7] ... [yy, 7]
4 [1, 1] [x, 7] ... [yy, 7] [52, 7] ... [101, 7]
```

`0`和`1`在一个区，2/3/4在另一个区，两个区合并后，0/1的`[2..51]`这些部分是都要被删去，并用`[x...yy]`这些重写的。而按照论文上写的，只用选择`LeaderCommit`和log长度中较小的一个作为覆盖终点即可，而由于2/3/4中的那个leader发给0/1两个节点的heartbeat中，entries是空的，且leaderCommit也是51，导致0/1的所有log全部被保留。

最后我的解决方式是：对于上面这个较小值，还要再和PrevLogIndex进行取min操作，以最后结果作为新的commitIndex

#### TestConcurrentStarts2B

因为同时提交了多个command，导致发起了大量并发的AppendEntries请求，如果两个请求次序如下


这个没问题

```conf
# 没问题
AppendEntries [1, 1]
AppendEntries [1, 1] [2, 1]
```


反过来不行
```conf
# 有问题
AppendEntries [1, 1] [2, 1]
AppendEntries [1, 1]
```

原因是对于entries非空的AppendEntries，都会根据PrevLogIndex进行截断然后最佳entries部分，如果对于同一个PrevLogIndex的追加，短的在后面，会将长的log截断，而长的先返回导致leader认为已经commit成功了，再往后面如果有新的请求，会因为匹配不上导致不符合预期。

我最后处理方法是对log的append操作先进行检查，短的，且再当前log中全都能找到的，不动log数组。

