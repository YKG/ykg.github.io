---
title: Linux性能优化-实验报告19
date: 2019-08-06 10:57:11
tags: [Perf]
---


```
root@perf:/home/ykg# numactl --hardware
available: 1 nodes (0)
node 0 cpus: 0 1
node 0 size: 7952 MB
node 0 free: 6841 MB
node distances:
node   0
  0:  10
```

```
root@perf:/home/ykg# cat /proc/zoneinfo
Node 0, zone      DMA
  per-node stats
      ...
  pages free     3977
        min      33
        low      41
        high     49
        spanned  4095
        present  3998
        managed  3977
        protection: (0, 925, 7912, 7912, 7912)
      ...
  pagesets
    cpu: 0
              count: 0
              high:  0
              batch: 1
  vm stats threshold: 4
    cpu: 1
              count: 0
              high:  0
              batch: 1
  vm stats threshold: 4
  node_unreclaimable:  0
  start_pfn:           1
Node 0, zone    DMA32
  pages free     241441
        min      1972
        low      2465
        high     2958
        spanned  1044480
        present  258032
        managed  241640
        protection: (0, 0, 6986, 6986, 6986)
      ...
  pagesets
    cpu: 0
              count: 29
              high:  186
              batch: 31
  vm stats threshold: 16
    cpu: 1
              count: 168
              high:  186
              batch: 31
  vm stats threshold: 16
  node_unreclaimable:  0
  start_pfn:           4096
Node 0, zone   Normal
  pages free     1506103
        min      14890
        low      18612
        high     22334
        spanned  1835008
        present  1835008
        managed  1790149
        protection: (0, 0, 0, 0, 0)
      nr_free_pages 1506103
      nr_zone_inactive_anon 77
      nr_zone_active_anon 36503
      nr_zone_inactive_file 57721
      nr_zone_active_file 111407
      nr_zone_unevictable 2644
      nr_zone_write_pending 25
      nr_mlock     2644
      nr_page_table_pages 1417
      nr_kernel_stack 4800
      nr_bounce    0
      nr_zspages   0
      nr_free_cma  0
      numa_hit     1990448
      numa_miss    0
      numa_foreign 0
      numa_interleave 18596
      numa_local   1990448
      numa_other   0
  pagesets
    cpu: 0
              count: 122
              high:  186
              batch: 31
  vm stats threshold: 28
    cpu: 1
              count: 94
              high:  186
              batch: 31
  vm stats threshold: 28
  node_unreclaimable:  0
  start_pfn:           1048576
Node 0, zone  Movable
  pages free     0
        min      0
        low      0
        high     0
        spanned  0
        present  0
        managed  0
        protection: (0, 0, 0, 0, 0)
Node 0, zone   Device
  pages free     0
        min      0
        low      0
        high     0
        spanned  0
        present  0
        managed  0
        protection: (0, 0, 0, 0, 0)
```

```
root@perf:/home/ykg# cat /proc/sys/vm/swappiness
60
```

# 20

```
root@perf:/home/ykg# free
              total        used        free      shared  buff/cache   available
Mem:        8143064      392216     7006908        1596      743940     7484920
Swap:             0           0           0
```


```
# 创建 Swap 文件
$ fallocate -l 8G /mnt/swapfile
# 修改权限只有根用户可以访问
$ chmod 600 /mnt/swapfile
# 配置 Swap 文件
$ mkswap /mnt/swapfile
# 开启 Swap
$ swapon /mnt/swapfile

root@perf:/home/ykg# free
              total        used        free      shared  buff/cache   available
Mem:        8143064      394932     7002068        1612      746064     7482188
Swap:       8388604           0     8388604
```

```
03:13:20    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:21       133536   6418972   8009528     98.36   5919184    563468   1581504      9.57   1275684   6398644         4

03:13:20    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:21      8388604         0      0.00         0      0.00

03:13:21    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:22       131488   6418960   8011576     98.39   5922188    562444   1581504      9.57   1275352   6400980         4

03:13:21    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:22      8388604         0      0.00         0      0.00

03:13:22    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:23       129100   6418944   8013964     98.41   5925700    561296   1581504      9.57   1275004   6403684         4

03:13:22    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:23      8388604         0      0.00         0      0.00

03:13:23    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:24       149620   6418968   7993444     98.16   5907104    559340   1581504      9.57   1274532   6383628         4

03:13:23    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:24      8388604         0      0.00         0      0.00

03:13:24    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:25       146952   6418864   7996112     98.20   5910612    558372   1581504      9.57   1274236   6386456         4

03:13:24    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:25      8388604         0      0.00         0      0.00

03:13:25    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:26       144656   6418796   7998408     98.22   5914112    557256   1591616      9.63   1273828   6389080         4

03:13:25    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:26      8388604         0      0.00         0      0.00

03:13:26    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:27       142888   6418980   8000176     98.25   5917096    556032   1591616      9.63   1273388   6391440         4

03:13:26    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:27      8388604         0      0.00         0      0.00

03:13:27    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:28       140096   6418832   8002968     98.28   5920592    555156   1591616      9.63   1273028   6394412         4

03:13:27    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:28      8388604         0      0.00         0      0.00

03:13:28    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:29       138792   6418844   8004272     98.30   5923056    553980   1591616      9.63   1272576   6396148         4

03:13:28    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:29      8388604         0      0.00         0      0.00

03:13:29    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:30       135868   6418848   8007196     98.33   5927068    552940   1591616      9.63   1272220   6399416         4

03:13:29    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:30      8388604         0      0.00         0      0.00

03:13:30    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:31       133240   6418664   8009824     98.36   5930548    551824   1591616      9.63   1271912   6402136         4

03:13:30    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:31      8388604         0      0.00         0      0.00

03:13:31    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:32       131316   6418716   8011748     98.39   5933508    550784   1591616      9.63   1271568   6404424         4

03:13:31    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:32      8388604         0      0.00         0      0.00

03:13:32    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:33       129020   6418712   8014044     98.42   5936988    549708   1591616      9.63   1271020   6407240         0

03:13:32    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:33      8388604         0      0.00         0      0.00

03:13:33    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:34       149572   6418864   7993492     98.16   5918840    547360   1591616      9.63   1270184   6387704         0

03:13:33    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:34      8388604         0      0.00         0      0.00

03:13:34    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:35       149696   6420720   7993368     98.16   5921784    546092   1591616      9.63   1269772   6389816         0

03:13:34    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:35      8388604         0      0.00         0      0.00

03:13:35    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:36       147172   6420324   7995892     98.19   5925232    544768   1591616      9.63   1269420   6392284         0

03:13:35    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:36      8388604         0      0.00         0      0.00

03:13:36    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:37       145508   6420604   7997556     98.21   5928680    543364   1591616      9.63   1268956   6394660         0

03:13:36    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:37      8388604         0      0.00         0      0.00

03:13:37    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:38       143832   6420628   7999232     98.23   5931604    541980   1591616      9.63   1268548   6396736         0

03:13:37    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:38      8388604         0      0.00         0      0.00

03:13:38    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:13:39       141908   6420576   8001156     98.26   5934912    540664   1591616      9.63   1268136   6399004         0

03:13:38    kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:13:39      8388604         0      0.00         0      0.00
```

%memused 不断在98.1到98.4之间循环


```
$ /usr/share/bcc/cachetop 5
03:15:45 Buffers MB: 5862 / Cached MB: 434 / Sort: HITS / Order: ascending
PID      UID      CMD              HITS     MISSES   DIRTIES  READ_HIT%  WRITE_HIT%
    1125 root     atopacctd               1        0        0     100.0%       0.0%
    1517 root     python3                 2        0        0     100.0%       0.0%
    9485 root     cachetop               13        0        0     100.0%       0.0%
    1476 root     python3                23        2        1      88.0%       4.0%
    9507 root     sh                    109        0        0     100.0%       0.0%
    9505 root     sh                    112        0        0     100.0%       0.0%
    9504 root     python3               366        0        0     100.0%       0.0%
    9506 root     python3               366        0        0     100.0%       0.0%
    9504 root     sh                    398        0        0     100.0%       0.0%
    9506 root     sh                    406        0        0     100.0%       0.0%
    9505 root     iptables              451        0        0     100.0%       0.0%
    9507 root     iptables              546        0        0     100.0%       0.0%
    9116 root     dd                  31491    31170        0      50.3%      49.7%
```



```
# -d 表示高亮变化的字段
# -A 表示仅显示 Normal 行以及之后的 15 行输出
$ watch -d grep -A 15 'Normal' /proc/zoneinfo

Every 2.0s: grep -A 15 Normal /proc/zoneinfo                                                    perf: Tue Aug  6 03:17:51 2019

Node 0, zone   Normal
  pages free     21038
        min      14890
        low      18612
        high     22334
        spanned  1835008
        present  1835008
        managed  1790149
        protection: (0, 0, 0, 0, 0)
      nr_free_pages 21038
      nr_zone_inactive_anon 86297
      nr_zone_active_anon 213185
      nr_zone_inactive_file 1329083
      nr_zone_active_file 55889
      nr_zone_unevictable 2700
      nr_zone_write_pending 6
```

可以看到free不断在(low+, high+)区间变化

```
# 按 VmSwap 使用量对进程排序，输出进程名称、进程 ID 以及 SWAP 用量
$ for file in /proc/*/status ; do awk '/VmSwap|Name|^Pid/{printf $2 " " $3}END{ print ""}' $file; done | sort -k 3 -n -r | head

dockerd 3409 316 kB
containerd 1322 124 kB
python3 1152 24 kB
writeback 30
watchdogd 47
watchdog/1 16
watchdog/0 13
unattended-upgr 1334 0 kB
systemd-udevd 475 0 kB
systemd-timesyn 720 0 kB
```


```
root@perf:/home/ykg# free
              total        used        free      shared  buff/cache   available
Mem:        8143064     1446952      158908        1764     6537204     6414524
Swap:       8388604        1036     8387568
root@perf:/home/ykg# free
              total        used        free      shared  buff/cache   available
Mem:        8143064     1447400      138480        1768     6557184     6414308
Swap:       8388604        1292     8387312
```

```
root@perf:/home/ykg# swapoff -a
root@perf:/home/ykg# free
              total        used        free      shared  buff/cache   available
Mem:        8143064     1445364      139580        1792     6558120     6413972
Swap:             0           0           0
```

看评论去评论，有人提到`smem --sort swap`可以按照swap使用量对进程排序
