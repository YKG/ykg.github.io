---
title: terminate process with SIGTERM
date: 2019-07-30 21:07:06
tags: [Linux]
---

kill -s SIGTERM <pid>

或者 kill -15 <pid>

> kill -15代表的信号为SIGTERM，这是告诉进程你需要被关闭，请自行停止运行并退出；  
而kill -9代表的信号是SIGKILL，表示进程被终止，需要立即退出；


[参考](https://www.jianshu.com/p/5729fc095b2a)
