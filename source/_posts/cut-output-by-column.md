---
title: 按列过滤输出
date: 2019-08-04 13:27:09
tags: [基本命令]
---

```bash
$ cat file.txt
abcde12345

$ cat file.txt | cut -b 1-3,5-6 # 切割第1到3列和5到6列
abc12
```

今天遇到的情况是在想查看Azure上一台2c8g机器的`/proc/softirqs`内容，结果打出来最高的cpu编号是`CPU127`，Azure这块没处理好。输出格式全乱了，所以切割出来前两个

```
ykg@perf:~$ cat /proc/softirqs |cut -b 1-50,1402-2000
                    CPU0       CPU1       CPU2         CPU126     CPU127
          HI:          0          0          0            0          0
       TIMER:     199367     263391          0            0          0
      NET_TX:         24         16          0            0          0
      NET_RX:      43705      47277          0            0          0
       BLOCK:          0          0          0            0          0
    IRQ_POLL:          0          0          0            0          0
     TASKLET:     136955      57164          0            0          0
       SCHED:     167598     224132          0            0          0
     HRTIMER:          0         12          0            0          0
         RCU:     185287     247755          0            0          0

ykg@perf:~$ cat /proc/softirqs |cut -b 1-40
                    CPU0       CPU1
          HI:          0          0
       TIMER:     199818     263899
      NET_TX:         24         16
      NET_RX:      43888      47473
       BLOCK:          0          0
    IRQ_POLL:          0          0
     TASKLET:     137483      57170
       SCHED:     168048     224646
     HRTIMER:          0         12
         RCU:     185653     248292         
```

开始想用awk输出的，但因为这个第一列表头是空的，导致有点错位。

```
ykg@perf:~$ cat /proc/softirqs |awk '{print $1,$2,$3}'
CPU0 CPU1 CPU2
HI: 0 0
TIMER: 210324 281783
NET_TX: 24 16
NET_RX: 48840 52825
BLOCK: 0 0
IRQ_POLL: 0 0
TASKLET: 148045 57400
SCHED: 178658 242629
HRTIMER: 0 12
RCU: 194608 266893
```
