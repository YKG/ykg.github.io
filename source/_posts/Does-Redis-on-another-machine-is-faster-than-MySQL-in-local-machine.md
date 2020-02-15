---
title: 网络连接的Redis是否优于本地的DB
date: 2020-01-31 11:18:58
tags: [Redis, 性能]
---


原先一直考虑到这个问题，网络延迟一般是要比磁盘读写慢的，那么放在网络上的Redis是否比本地DB访问速度更快呢？

看了下知乎也有人提出[这个问题][1]。

看了有些人的回答，一般情况下，DB也不会放在App同一个机器上，所以对比应该只是memory和DB读写的延迟对比，明显是redis更优的。

所以题目上的问题一般情况下（生产环境）是不会出现的。只能作为一个理论问题进行讨论。进而发现了提到《Designs, Lessons and Advice from Building Large Distributed Systems》Jeff Dean 2009年的一个ppt，讨论了一些延迟。可对比我看《性能之巅》时的一个[摘录][2]。


[1]: https://www.zhihu.com/question/47589908
[2]: http://kaige.org/2019/08/02/a-list-of-latency/
