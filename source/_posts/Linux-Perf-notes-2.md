---
title: Linux性能优化-实验报告04
date: 2019-08-03 12:13:08
tags: [Perf]
---

这个实验用来学习上下文切换

```
apt install sysbench
```

```
root@perf:/home/ykg# vmstat 1 1
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 7365884  25792 440260    0    0    17   542   97  543 13  1 81  5  0
```

```
# 以 10 个线程运行 5 分钟的基准测试，模拟多线程切换的问题
sysbench --threads=10 --max-time=300 threads run
```

```
ykg@perf:~$ vmstat 1
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 7363852  25852 440316    0    0    17   533   95  533 13  1 82  5  0
 0  0      0 7363844  25852 440316    0    0     0     4   20   89  0  0 100  0  0
...
 0  0      0 7363512  25860 440324    0    0     0     4   20   99  0  0 100  0  0
 0  0      0 7363520  25860 440324    0    0     0    20   12   47  0  0 100  0  0
 0  0      0 7363520  25860 440324    0    0     0     0   14   55  0  0 100  0  0
 0  0      0 7363520  25860 440324    0    0     0     4   23  100  0  0 100  0  0
 0  0      0 7363636  25860 440324    0    0     0     0   29  107  1  0 100  0  0
 8  0      0 7361156  25860 440580    0    0   256     0 13510 806968 21 53 27  0  0
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 9  0      0 7361156  25860 440580    0    0     0     4 11897 1146131 35 66  0  0  0
 7  0      0 7361156  25860 440580    0    0     0     0 15220 1143409 33 67  0  0  0
 6  0      0 7361156  25860 440580    0    0     0     0 11226 1131061 36 64  0  0  0
 8  0      0 7360256  25860 440580    0    0     0     4 17465 1075562 35 65  0  0  0
 5  0      0 7360288  25860 440580    0    0     0     0 17061 1100762 30 70  0  0  0
```

可以看到cs/in的值明显飙升

```
# pidstat -w -u 1
04:59:18      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
04:59:19        0     28388   71.00  100.00    0.00    0.00  100.00     0  sysbench

04:59:18      UID       PID   cswch/s nvcswch/s  Command
04:59:19        0         9      1.00      0.00  ksoftirqd/0
04:59:19        0        10     13.00      0.00  rcu_sched
04:59:19        0        13      1.00      0.00  watchdog/0
04:59:19        0        16      1.00      0.00  watchdog/1
04:59:19        0       212      1.00      0.00  kworker/0:3-events
04:59:19        0       498      1.00      0.00  hv_balloon
04:59:19        0      1118      1.00      0.00  irqbalance
04:59:19        0     27306    124.00      0.00  kworker/u256:3-events_power_efficient
04:59:19     1000     27872    110.00      0.00  sshd
04:59:19     1000     28365      1.00      3.00  vmstat
04:59:19        0     28671      1.00    108.00  pidstat
```

可以看到sysbench占用了100% cpu（不过很奇怪%usr + %sys == 171%是什么情况？）。pidstat的值加起来也不高，明显达不到vmstat输出的情况。

因为pidstat统计的是按照进程切换，`-t`参数显示线程

```
# pidstat -w -u 1 -t # -t Also display statistics for threads associated with selected tasks.
05:08:58      UID      TGID       TID    %usr %system  %guest   %wait    %CPU   CPU  Command
05:08:59        0         -      1326    0.00    1.00    0.00    0.00    1.00     1  |__python3
05:08:59     1000     27872         -    1.00    0.00    0.00    0.00    1.00     0  sshd
05:08:59     1000         -     27872    1.00    0.00    0.00    0.00    1.00     0  |__sshd
05:08:59        0     29185         -   72.00  100.00    0.00    0.00  100.00     1  sysbench
05:08:59        0         -     29186    9.00   11.00    0.00   53.00   20.00     1  |__sysbench
05:08:59        0         -     29187    8.00   11.00    0.00   49.00   19.00     0  |__sysbench
05:08:59        0         -     29188    7.00   13.00    0.00   47.00   20.00     0  |__sysbench
05:08:59        0         -     29189    5.00   14.00    0.00   38.00   19.00     0  |__sysbench
05:08:59        0         -     29190    8.00   12.00    0.00   54.00   20.00     1  |__sysbench
05:08:59        0         -     29191    8.00   12.00    0.00   56.00   20.00     1  |__sysbench
05:08:59        0         -     29192    6.00   14.00    0.00   57.00   20.00     0  |__sysbench
05:08:59        0         -     29193    7.00   13.00    0.00   48.00   20.00     1  |__sysbench
05:08:59        0         -     29194    8.00   13.00    0.00   57.00   21.00     0  |__sysbench
05:08:59        0         -     29195    6.00   12.00    0.00   54.00   18.00     1  |__sysbench
05:08:59        0     29209         -    0.00    1.00    0.00    7.00    1.00     1  pidstat
05:08:59        0         -     29209    0.00    1.00    0.00    7.00    1.00     1  |__pidstat

05:08:58      UID      TGID       TID   cswch/s nvcswch/s  Command
05:08:59        0         9         -      1.00      0.00  ksoftirqd/0
05:08:59        0         -         9      1.00      0.00  |__ksoftirqd/0
05:08:59        0        10         -     23.00      0.00  rcu_sched
05:08:59        0         -        10     23.00      0.00  |__rcu_sched
05:08:59        0        18         -      3.00      0.00  ksoftirqd/1
05:08:59        0         -        18      3.00      0.00  |__ksoftirqd/1
05:08:59        0       123         -      1.00      0.00  kworker/0:1H-kblockd
05:08:59        0         -       123      1.00      0.00  |__kworker/0:1H-kblockd
05:08:59        0       212         -      1.00      0.00  kworker/0:3-mm_percpu_wq
05:08:59        0         -       212      1.00      0.00  |__kworker/0:3-mm_percpu_wq
05:08:59        0       498         -      1.00      0.00  hv_balloon
05:08:59        0         -       498      1.00      0.00  |__hv_balloon
05:08:59        0      1326         -      9.00      0.00  python3
05:08:59        0         -      1326      9.00      0.00  |__python3
05:08:59        0     27305         -      1.00      0.00  kworker/1:1-mm_percpu_wq
05:08:59        0         -     27305      1.00      0.00  |__kworker/1:1-mm_percpu_wq
05:08:59        0     27306         -     51.00      0.00  kworker/u256:3-events_power_efficient
05:08:59        0         -     27306     51.00      0.00  |__kworker/u256:3-events_power_efficient
05:08:59     1000     27872         -    278.00     59.00  sshd
05:08:59     1000         -     27872    278.00     59.00  |__sshd
05:08:59        0     28931         -    304.00      0.00  kworker/u256:1-events_unbound
05:08:59        0         -     28931    304.00      0.00  |__kworker/u256:1-events_unbound
05:08:59        0         -     29186  18792.00  98414.00  |__sysbench
05:08:59        0         -     29187  18737.00  97043.00  |__sysbench
05:08:59        0         -     29188  23418.00  95042.00  |__sysbench
05:08:59        0         -     29189  23430.00  82341.00  |__sysbench
05:08:59        0         -     29190  18268.00  95634.00  |__sysbench
05:08:59        0         -     29191  14390.00 104050.00  |__sysbench
05:08:59        0         -     29192  19305.00  97544.00  |__sysbench
05:08:59        0         -     29193  20488.00  92663.00  |__sysbench
05:08:59        0         -     29194  18648.00 102499.00  |__sysbench
05:08:59        0         -     29195  14792.00 103681.00  |__sysbench
05:08:59     1000     29204         -      1.00      0.00  vmstat
05:08:59     1000         -     29204      1.00      0.00  |__vmstat
05:08:59        0     29209         -      1.00    279.00  pidstat
05:08:59        0         -     29209      1.00    279.00  |__pidstat
```


大量的interrupt变动发生在RES上，IPI处理器间调度，在SMP中用来将任务分散到不同cpu的机制

```
Every 2.0s: cat /proc/interrupts                                   perf: Sat Aug  3 06:00:55 2019

            CPU0       CPU1
   0:         46          0   IO-APIC    2-edge      timer
   1:          0          9   IO-APIC    1-edge      i8042
   4:          0       1599   IO-APIC    4-edge      ttyS0
   8:          0          0   IO-APIC    8-edge      rtc0
   9:          0          0   IO-APIC    9-fasteoi   acpi
  12:          3          0   IO-APIC   12-edge      i8042
  14:          0          0   IO-APIC   14-edge      ata_piix
  15:          0          0   IO-APIC   15-edge      ata_piix
 NMI:          0          0   Non-maskable interrupts
 LOC:        142         89   Local timer interrupts
 SPU:          0          0   Spurious interrupts
 PMI:          0          0   Performance monitoring interrupts
 IWI:          1          0   IRQ work interrupts
 RTR:          0          0   APIC ICR read retries
 RES:    7839865    9962474   Rescheduling interrupts
 CAL:      50752       2521   Function call interrupts
 TLB:          0          0   TLB shootdowns
 TRM:          0          0   Thermal event interrupts
 THR:          0          0   Threshold APIC interrupts
 DFR:          0          0   Deferred Error APIC interrupts
 MCE:          0          0   Machine check exceptions
 MCP:         49         49   Machine check polls
 HYP:    2631487      82131   Hypervisor callback interrupts
 HRE:          0          0   Hyper-V reenlightenment interrupts
 HVS:     871419    1002514   Hyper-V stimer0 interrupts
 ERR:          0
 MIS:          0
 PIN:          0          0   Posted-interrupt notification event
 NPI:          0          0   Nested posted-interrupt event
 PIW:          0          0   Posted-interrupt wakeup event
```

