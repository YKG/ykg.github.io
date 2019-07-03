---
title: how Desktop for Mac works
date: 2019-07-03 20:12:17
tags:
---

因为docker的运行需要有linux的containerlib支持，但win/osx都没有这个基础库，所以这个Desktop是使用Hypervisor技术（xhyve）运行了一个Linux distribution，相当于起了一个linux虚拟机来为docker提供一个runtime。

参考：[Let me explain Docker for Mac in a little more detail][1]


[1]: https://news.ycombinator.com/item?id=11352594
