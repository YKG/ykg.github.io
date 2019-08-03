---
title: Linux性能优化-实验报告02
date: 2019-08-03 09:27:58
tags: [Perf]
---

跟着极客时间上买的《Linux性能优化实战》课程，在Azure上新建一个2c8g的ubuntu 18.04

atop一看这个b2ms型、max iops 2400的机器，avio还是高达几十ms到200+ms,对比最早的一个sgp机器，avio只有1～4ms。而那台hk机器，avio高达上千ms。换到max iops 3200的，avio居然到2000+ms了，还是换回2400的，省点钱。

安装 stress 和 sysstat

```
sudo apt install stress sysstat
```

分别做了3个实验，观测手段：

```bash
watch -d uptime # -d 可以高亮变化部分
mpstat -P ALL 5 # 每5秒输出所有cpu的使用情况
pidstat -u 5 1  # 间隔5秒输出1份cpu使用(-u)报告，报告活动的task统计
```


共享实验前

```
# watch -d uptime
Every 2.0s: uptime                                                  perf: Sat Aug  3 03:36:56 2019

 03:36:56 up  1:48,  4 users,  load average: 0.00, 0.00, 0.06
```

```
root@perf:/home/ykg# mpstat -P ALL 5
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:41:31     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:41:36     all    0.20    0.00    0.10    0.10    0.00    0.10    0.00    0.00    0.00   99.50
03:41:36       0    0.00    0.00    0.00    0.00    0.00    0.20    0.00    0.00    0.00   99.80
03:41:36       1    0.20    0.00    0.20    0.00    0.00    0.00    0.00    0.00    0.00   99.60
```

```
root@perf:/home/ykg# pidstat -u 5 1
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:42:05      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:42:10        0      1326    0.20    0.00    0.00    0.00    0.20     0  python3
03:42:10        0      3101    0.20    0.00    0.00    0.00    0.20     0  watch

Average:      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
Average:        0      1326    0.20    0.00    0.00    0.00    0.20     -  python3
Average:        0      3101    0.20    0.00    0.00    0.00    0.20     -  watch
```

- 实验1: cpu密集型

```bash
stress --cpu 1 --timeout 600 # -c, --cpu N spawn N workers spinning on sqrt()
                             # -t, --timeout N timeout after N seconds
                             # 让一个核占用100%，持续600s
```

实验后

```
# watch -d uptime
Every 2.0s: uptime                                                  perf: Sat Aug  3 03:40:54 2019

 03:40:54 up  1:52,  4 users,  load average: 1.07, 0.51, 0.25
```

```
root@perf:/home/ykg# mpstat -P ALL 5
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:38:01     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:38:06     all   50.20    0.00    0.10    0.20    0.00    0.00    0.00    0.00    0.00   49.50
03:38:06       0    0.60    0.00    0.20    0.40    0.00    0.00    0.00    0.00    0.00   98.80
03:38:06       1  100.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00
```

```
root@perf:/home/ykg# pidstat -u 5 1
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:39:34      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:39:39        0      1326    0.00    0.40    0.00    0.20    0.40     0  python3
03:39:39        0      3101    0.20    0.00    0.00    0.00    0.20     0  watch
03:39:39        0     18067  100.00    0.00    0.00    0.00  100.00     1  stress

Average:      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
Average:        0      1326    0.00    0.40    0.00    0.20    0.40     -  python3
Average:        0      3101    0.20    0.00    0.00    0.00    0.20     -  watch
Average:        0     18067  100.00    0.00    0.00    0.00  100.00     -  stress
```


- 实验2: i/o密集型

```bash
stress -i 1 --timeout 600   # -i, --io N spawn N workers spinning on sync()
                            # 
```

实验后

```
# watch -d uptime
Every 2.0s: uptime                                                  perf: Sat Aug  3 03:49:10 2019

 03:49:10 up  2:00,  4 users,  load average: 1.07, 0.81, 0.49
```

```
# mpstat -P ALL 5
03:43:22     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:43:27     all    0.10    0.00    0.20    0.00    0.00    0.00    0.00    0.00    0.00   99.70
03:43:27       0    0.20    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00   99.80
03:43:27       1    0.00    0.00    0.20    0.00    0.00    0.00    0.00    0.00    0.00   99.80

03:43:27     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:43:32     all    0.49    0.00    6.32    5.54    0.00    0.49    0.00    0.00    0.00   87.16
03:43:32       0    0.00    0.00    2.22   10.91    0.00    1.01    0.00    0.00    0.00   85.86
03:43:32       1    0.75    0.00   10.34    0.38    0.00    0.00    0.00    0.00    0.00   88.53

03:43:32     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:43:37     all    0.58    0.00    9.60   27.26    0.00    0.48    0.00    0.00    0.00   62.09
03:43:37       0    0.77    0.00   11.37   54.34    0.00    0.77    0.00    0.00    0.00   32.76
03:43:37       1    0.38    0.00    7.66    0.38    0.00    0.19    0.00    0.00    0.00   91.38

03:43:37     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:43:42     all    0.40    0.00    6.65    9.33    0.00    0.89    0.00    0.00    0.00   82.72
03:43:42       0    0.20    0.00    4.00   18.40    0.00    1.60    0.00    0.00    0.00   75.80
03:43:42       1    0.78    0.00    9.41    0.59    0.00    0.00    0.00    0.00    0.00   89.22

03:43:42     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:43:47     all    0.40    0.00    4.12   31.29    0.00    1.11    0.00    0.00    0.00   63.08
03:43:47       0    0.40    0.00    5.86   62.02    0.00    2.02    0.00    0.00    0.00   29.70
03:43:47       1    0.40    0.00    2.40    0.80    0.00    0.20    0.00    0.00    0.00   96.19
```

%iowait似乎并不够高

```
# pidstat -u 5
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:44:24      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:44:29        0       117    0.00    1.80    0.00    0.00    1.80     1  kworker/1:1H-kblockd
03:44:29        0      1326    0.20    0.20    0.00    0.00    0.40     0  python3
03:44:29        0     18908    0.60   14.97    0.00    3.59   15.57     1  stress
```

> iowait无法升高的问题，是因为案例中stress使用的是 sync() 系统调用，它的作用是刷新缓冲区内存到磁盘中。对于新安装的虚拟机，缓冲区可能比较小，无法产生大的IO压力，这样大部分就都是系统调用的消耗了。所以，你会看到只有系统CPU使用率升高。解决方法是使用stress的下一代stress-ng，它支持更丰富的选项，比如 stress-ng -i 1 --hdd 1 --timeout 600（--hdd表示读写临时文件）。——倪鹏飞

使用stress-ng之后

```bash
stress-ng -i 1 --hdd 1 --timeout 600 # 没弄明白为什么下面的负载会到4.5，期望的是1。
                                     # mpstat多个cpu wait都是接近100%，期望只有1个。
                                     # pidstat的结果也是没有高的
                                     #
                                     # 另外就是这个命令ctrl+c要等一段时间才能停止。
```

```
# watch -d uptime
Every 2.0s: uptime                                                  perf: Sat Aug  3 04:06:21 2019

 04:06:21 up  2:17,  4 users,  load average: 4.50, 2.88, 2.58
```


```
# mpstat -P ALL 5
03:55:17     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:55:22     all    0.20    0.00    0.50    0.00    0.00    0.00    0.00    0.00    0.00   99.30
03:55:22       0    0.20    0.00    0.60    0.00    0.00    0.00    0.00    0.00    0.00   99.20
03:55:22       1    0.00    0.00    0.60    0.00    0.00    0.00    0.00    0.00    0.00   99.40

03:55:22     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:55:27     all    0.20    0.00    5.62   79.04    0.00    0.00    0.00    0.00    0.00   15.15
03:55:27       0    0.20    0.00    0.60   96.78    0.00    0.00    0.00    0.00    0.00    2.41
03:55:27       1    0.40    0.00   10.40   61.20    0.00    0.00    0.00    0.00    0.00   28.00

03:55:27     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:55:32     all    0.20    0.00    4.84   93.44    0.00    0.00    0.00    0.00    0.00    1.51
03:55:32       0    0.20    0.00    0.61   98.99    0.00    0.00    0.00    0.00    0.00    0.20
03:55:32       1    0.20    0.00    9.24   87.75    0.00    0.00    0.00    0.00    0.00    2.81

03:55:32     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:55:37     all    0.10    0.00    0.20   99.70    0.00    0.00    0.00    0.00    0.00    0.00
03:55:37       0    0.00    0.00    0.20   99.80    0.00    0.00    0.00    0.00    0.00    0.00
03:55:37       1    0.00    0.00    0.00  100.00    0.00    0.00    0.00    0.00    0.00    0.00

03:55:37     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:55:42     all    0.00    0.00    0.20   99.80    0.00    0.00    0.00    0.00    0.00    0.00
03:55:42       0    0.00    0.00    0.20   99.80    0.00    0.00    0.00    0.00    0.00    0.00
03:55:42       1    0.00    0.00    0.20   99.80    0.00    0.00    0.00    0.00    0.00    0.00
```


```
root@perf:/home/ykg# pidstat -u 5
Linux 4.18.0-1024-azure (perf) 	08/03/19 	_x86_64_	(2 CPU)

03:56:27      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:32        0     20422    0.00    0.20    0.00    0.00    0.20     1  kworker/u256:0-events_unbound
03:56:32        0     20584    0.20    0.00    0.00    0.00    0.20     0  watch
03:56:32        0     20995    0.00    0.20    0.00    0.00    0.20     0  pidstat

03:56:32      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:37        0      1326    0.00    0.20    0.00    0.00    0.20     0  python3

03:56:37      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:42        0     20584    0.20    0.00    0.00    0.00    0.20     0  watch

03:56:42      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:47        0      1326    0.00    0.20    0.00    0.00    0.20     0  python3
03:56:47        0     20995    0.20    0.00    0.00    0.00    0.20     0  pidstat

03:56:47      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:52        0     18615    0.00    0.40    0.00    0.00    0.40     0  kworker/u256:2+flush-8:0
03:56:52        0     20584    0.20    0.00    0.00    0.00    0.20     1  watch
03:56:52        0     20825    0.00    0.60    0.00    0.00    0.60     1  stress-ng-hdd
03:56:52        0     20995    0.00    0.20    0.00    0.00    0.20     0  pidstat

03:56:52      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:56:57        0       373    0.00    0.20    0.00    0.00    0.20     1  jbd2/sda1-8
03:56:57        0       458    0.00    0.80    0.00    0.00    0.80     1  hv_kvp_daemon
03:56:57        0      1326    0.00    0.20    0.00    0.00    0.20     0  python3
03:56:57        0     20584    0.00    0.20    0.00    0.00    0.20     1  watch
03:56:57        0     20995    0.20    0.00    0.00    0.00    0.20     0  pidstat

03:56:57      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:57:02        0        10    0.00    0.20    0.00    0.20    0.20     0  rcu_sched
03:57:02        0      1326    0.20    0.00    0.00    0.00    0.20     0  python3
03:57:02     1000      1820    0.00    0.20    0.00    0.00    0.20     1  sshd
03:57:02        0     18615    0.00    0.40    0.00    0.00    0.40     0  kworker/u256:2-events_power_efficient
03:57:02        0     20584    0.20    0.00    0.00    0.00    0.20     1  watch
03:57:02        0     20825    0.00    3.00    0.00    0.00    3.00     1  stress-ng-hdd
```


- 实验3: 大量进程

```
stress -c 8 --timeout 600
```

```
# watch -d uptime
Every 2.0s: uptime                                                  perf: Sat Aug  3 03:54:41 2019

 03:54:41 up  2:06,  4 users,  load average: 8.03, 5.36, 2.56
```


```
# mpstat -P ALL 5
03:49:57     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
03:50:02     all   99.90    0.00    0.00    0.00    0.00    0.10    0.00    0.00    0.00    0.00
03:50:02       0  100.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00
03:50:02       1   99.60    0.00    0.20    0.00    0.00    0.20    0.00    0.00    0.00    0.00
```

```
#pidstat -u 5
03:50:24      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
03:50:29        0      1326    0.00    0.20    0.00    0.00    0.20     1  python3
03:50:29     1000      1820    0.00    0.20    0.00    0.00    0.20     0  sshd
03:50:29        0     19048    0.00    0.20    0.00    0.20    0.20     1  pidstat
03:50:29        0     19823   24.80    0.00    0.00   75.20   24.80     1  stress
03:50:29        0     19824   25.00    0.00    0.00   75.20   25.00     0  stress
03:50:29        0     19825   25.00    0.00    0.00   75.00   25.00     0  stress
03:50:29        0     19826   24.80    0.00    0.00   75.20   24.80     1  stress
03:50:29        0     19827   24.80    0.00    0.00   75.00   24.80     0  stress
03:50:29        0     19828   24.80    0.00    0.00   75.20   24.80     1  stress
03:50:29        0     19829   24.80    0.00    0.00   74.80   24.80     1  stress
03:50:29        0     19830   24.80    0.00    0.00   75.20   24.80     0  stress
```


参考：[02 | 基础篇：到底应该怎么理解“平均负载”？](https://time.geekbang.org/column/article/69618)
