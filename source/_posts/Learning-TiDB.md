---
title: TiDB学习笔记1
date: 2019-05-31 22:05:20
tags: [TiDB]
---

总体上和前段时间看的分布式存储那里写的东西有些像，都是由划分64MB大小区块，有个区块管理机构，TiDB称之为PD(不知为啥叫这名)，角色记得对应别的分布式系统的MataData管理机构。之前看过TiDB的几个产品介绍，也知道TiKV这么个名字，没想到原来TiKV是最底层一级，DB是在KV基础上搭建的，我原来以为是两套独立的数据库产品呢（TiKV作为文档型DB或者对象存储之类的用）。看了《三篇文章了解TiDB》的[说存储][1]和[说计算][2]，调度部分比较散，没怎么看

- TiDB构建与TiKV之上
- 利用RocksDB做单机磁盘适配（RocksDB是Facebook开发维护）
- 使用优化版的Raft对单机系统进行分布式化（前段时间看到一篇文章提到国外有个女研究生发表了一篇新的共识协议，说是证明了是最优算法）([参考1][3] [参考2][4])





[1]: https://pingcap.com/blog-cn/tidb-internal-1/
[2]: https://pingcap.com/blog-cn/tidb-internal-2/
[3]: http://hh360.user.srcf.net/blog/publications/
[4]: https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-935.pdf
