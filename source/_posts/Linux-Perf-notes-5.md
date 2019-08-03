---
title: Linux性能优化-实验报告07
date: 2019-08-03 18:12:20
tags: [Perf]
---


```
docker run --privileged --name=app -itd feisky/app:iowait
```

```
root@perf:/home/ykg# ps aux|grep app
root     129834  0.1  0.0   4512   764 pts/0    Ss+  10:30   0:00 /app
root     129889  0.3  0.8  70052 65840 pts/0    D+   10:30   0:00 /app
root     129890  0.3  0.8  70052 65840 pts/0    D+   10:30   0:00 /app
root     129900  2.0  0.8  70052 65836 pts/0    D+   10:30   0:00 /app
root     129901  2.0  0.8  70052 65828 pts/0    D+   10:30   0:00 /app
root     129903  0.0  0.0  14856  1096 pts/0    S+   10:30   0:00 grep --color=auto app
```

S: 中断睡眠状态

D: 不可中断睡眠状态

s: session leader

+: is in the foreground process group

进程组表示一组相互关联的进程，比如每个子进程都是父进程所在组的成员

会话是指共享同一终端的一个或多个进程组
