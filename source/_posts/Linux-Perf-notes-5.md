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

```
top - 02:33:31 up 7 min,  1 user,  load average: 55.92, 21.33, 7.94
Tasks: 206 total,   1 running, 147 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.3 us,  0.3 sy,  0.0 ni,  0.0 id, 99.0 wa,  0.0 hi,  0.3 si,  0.0 st
%Cpu1  :  0.0 us,  0.0 sy,  0.0 ni,  0.0 id,100.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8143064 total,  3897696 free,  3639180 used,   606188 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  4230752 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  3164 root      20   0   70052  33332    256 D   0.3  0.4   0:00.01 app
     1 root      20   0   77960   9016   6560 S   0.0  0.1   0:03.40 systemd
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd
     3 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 rcu_gp
     4 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 rcu_par_gp
     6 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kworker/0:0H-kb
     7 root      20   0       0      0      0 I   0.0  0.0   0:00.06 kworker/u256:0-
     8 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 mm_percpu_wq
```

看到iowait都接近100%，负载高达几十，但是没有实验参考的zombie数量增加

```
root@perf:/home/ykg# dstat 1 10
You did not select any stats, using -cdngy by default.
--total-cpu-usage-- -dsk/total- -net/total- ---paging-- ---system--
usr sys idl wai stl| read  writ| recv  send|  in   out | int   csw
  1   1  33  65   0|  15M   25M|   0     0 |   0     0 |  65   339
  0   0   0 100   0|  19M    0 | 132B  310B|   0     0 |  16   161
  1   2   0  98   0|  22M 4096B|5840B 4479B|   0     0 |  49   305
  0   0   0 100   0|  22M    0 |  66B  150B|   0     0 |  19   181
  0   1   0  99   0|  22M    0 |  66B  118B|   0     0 |  41   261
  0   0   0 100   0|  22M 4096B|5840B 4447B|   0     0 |  28   204
  0   1   0  99   0|  22M    0 |  66B  118B|   0     0 |  21   194
  0   1   0  99   0|  22M    0 |  66B  110B|   0     0 |  19   197
  1   1   0  99   0|  22M 4096B|5840B 4439B|   0     0 |  22   225
  0   1   0  99   0|  22M    0 |  66B  118B|   0     0 |  44   259
```

执行top命令，找到一个pid为3419的状态为D的app进程。操作已经非常卡顿，响应时间很长。

```
root@perf:/home/ykg# pidstat -d -p 3419 1 3
Linux 4.18.0-1025-azure (perf) 	08/04/19 	_x86_64_	(2 CPU)

02:41:12      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:41:14        0      3419   2048.00      0.00      0.00     201  app
02:41:15        0      3419      0.00      0.00      0.00       0  app
02:41:16        0      3419      0.00      0.00      0.00     370  app
Average:        0      3419    531.95      0.00      0.00     190  app
```

执行

```
pidstat -d 1 20
```

只看到输出大量内容，大量的app进程、appport进程。有IO read，但不高，大都在几百kB，少量是在1k kB ～ 4k kB。没有出现作者给出的例子中的那样高达32M的。


```
root@perf:/home/ykg# strace -p 6569
strace: Process 6569 attached
```

无法复现实验中的情况

perf record -g
perf report

![](perf.png)

```
# 首先删除原来的应用
$ docker rm -f app
# 运行新的应用
$ docker run --privileged --name=app -itd feisky/app:iowait-fix1
```

![](zombie.png)

```
root@perf:/home/ykg# pstree -aps 13122
systemd,1
  └─containerd,1292
      └─containerd-shim,12660 -namespace moby -workdir...
          └─app,12683
              └─(app,13122)
```


```
$ docker rm -f app
$ docker run --privileged --name=app -itd feisky/app:iowait-fix2
```


