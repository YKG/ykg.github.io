---
title: Linux性能优化-实验报告27
date: 2019-08-13 10:07:11
tags: [Perf]
---

```
docker run --name=app -p 10000:80 -itd feisky/word-pop 

```

```
# iostat -d -x 1
Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00   57.00      0.00  27116.00     0.00     0.00   0.00   0.00    0.00 18937.33 428.14     0.00   475.72  17.33  98.80
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00   58.00      0.00  27188.00     0.00     0.00   0.00   0.00    0.00 19924.48 370.83     0.00   468.76  17.03  98.80
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
```


```
root@perf:/home/ykg# pidstat -d 1
Linux 4.18.0-1025-azure (perf) 	08/13/19 	_x86_64_	(2 CPU)

02:21:15      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:16        0       376      0.00    364.00      0.00    1244  jbd2/sda1-8
02:21:16        0      1376      0.00     20.00      0.00       0  atop
02:21:16        0      1523      0.00      0.00      0.00     897  python3
02:21:16        0      6370      0.00     84.00      0.00       0  python
02:21:16        0      6474      0.00      0.00      0.00      85  kworker/u256:0+flush-8:0

02:21:16      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:17        0      6370      0.00     80.00      0.00       0  python

02:21:17      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:18        0      6370      0.00    132.00      0.00       0  python

02:21:18      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:19        0      1523      0.00      4.00      0.00       0  python3
02:21:19        0      6370      0.00    356.00      0.00       0  python

02:21:19      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:20        0      6370      0.00    456.00      0.00       0  python

02:21:20      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:21        0      6370      0.00    668.00      0.00       0  python

02:21:21      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:22        0      6370      0.00    824.00      0.00       0  python

02:21:22      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:23        0      6370      0.00   1536.00      0.00       0  python

02:21:23      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:24        0      6370      0.00   3328.00      0.00       0  python

02:21:24      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:25        0      6370      0.00   9216.00      0.00       0  python

02:21:25      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:26        0      6370      0.00  30124.00      0.00       0  python

02:21:26      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:27        0      6370      0.00  41784.00      0.00       0  python

02:21:27      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
02:21:28        0      6370      0.00  15568.00      0.00       0  python
```

```
# strace -p 6370
...
stat("/usr/local/lib/python3.7/stringprep.py", {st_mode=S_IFREG|0644, st_size=12917, ...}) = 0
stat("/usr/local/lib/python3.7/_bootlocale.py", {st_mode=S_IFREG|0644, st_size=1801, ...}) = 0
stat("/usr/local/lib/python3.7/_bootlocale.py", {st_mode=S_IFREG|0644, st_size=1801, ...}) = 0
stat("/usr/local/lib/python3.7/_bootlocale.py", {st_mode=S_IFREG|0644, st_size=1801, ...}) = 0
select(0, NULL, NULL, NULL, {tv_sec=1, tv_usec=0}) = 0 (Timeout)
stat("/usr/local/lib/python3.7/importlib/_bootstrap.py", {st_mode=S_IFREG|0644, st_size=39278, ...}) = 0
stat("/usr/local/lib/python3.7/importlib/_bootstrap.py", {st_mode=S_IFREG|0644, st_size=39278, ...}) = 0
stat("/usr/local/lib/python3.7/importlib/_bootstrap.py", {st_mode=S_IFREG|0644, st_size=39278, ...}) = 0
stat("/usr/local/lib/python3.7/importlib/_bootstrap_external.py", {st_mode=S_IFREG|0644, st_size=59012, ...}) = 0
stat("/usr/local/lib/python3.7/importlib/_bootstrap_external.py", {st_mode=S_IFREG|0644, st_size=59012, ...}) = 0
...
```


```
root@perf:/usr/share/bcc/tools# ./filetop -C
Tracing... Output every 1 secs. Hit Ctrl-C to end

02:25:24 loadavg: 4.52 3.94 2.39 3/184 7319

TID    COMM             READS  WRITES R_Kb    W_Kb    T FILE
7319   python           0      1      0       6103    R 199.txt
7319   python           0      1      0       4882    R 232.txt
7319   python           0      1      0       4882    R 240.txt
7319   python           0      1      0       4687    R 160.txt
7319   python           0      1      0       4687    R 179.txt
7319   python           0      1      0       4638    R 171.txt
7319   python           0      1      0       4443    R 215.txt
7319   python           0      1      0       4443    R 227.txt
7319   python           0      1      0       4345    R 249.txt
7319   python           0      1      0       4345    R 233.txt
7319   python           0      1      0       4296    R 190.txt
7319   python           0      1      0       4101    R 225.txt
7319   python           0      1      0       4101    R 191.txt
7319   python           0      1      0       4003    R 248.txt
7319   python           0      1      0       3955    R 181.txt
7319   python           0      1      0       3857    R 231.txt
7319   python           0      1      0       3857    R 196.txt
7319   python           0      1      0       3808    R 168.txt
7319   python           0      1      0       3515    R 159.txt
7319   python           0      1      0       3515    R 238.txt

02:25:25 loadavg: 4.52 3.94 2.39 4/184 7323

TID    COMM             READS  WRITES R_Kb    W_Kb    T FILE
7319   python           0      1      0       5957    R 359.txt
7319   python           0      1      0       5712    R 340.txt
7319   python           0      1      0       4833    R 352.txt
7319   python           0      1      0       4443    R 347.txt
7319   python           0      1      0       4345    R 367.txt
7319   python           0      1      0       4345    R 346.txt
7319   python           0      1      0       4248    R 284.txt
7319   python           0      1      0       4248    R 321.txt
7319   python           0      1      0       4199    R 376.txt
7319   python           0      1      0       4101    R 323.txt
7319   python           0      1      0       4003    R 370.txt
7319   python           0      1      0       3955    R 285.txt
7319   python           0      1      0       3906    R 371.txt
7319   python           0      1      0       3759    R 287.txt
7319   python           0      1      0       3710    R 273.txt
7319   python           0      1      0       3662    R 351.txt
7319   python           0      1      0       3662    R 269.txt
7319   python           0      1      0       3613    R 314.txt
7319   python           0      1      0       3564    R 341.txt
7319   python           0      1      0       3564    R 274.txt

02:25:26 loadavg: 4.52 3.94 2.39 3/184 7323

TID    COMM             READS  WRITES R_Kb    W_Kb    T FILE
7319   python           0      1      0       5712    R 383.txt
7319   python           0      1      0       5224    R 409.txt
7319   python           0      1      0       4541    R 411.txt
7319   python           0      1      0       4443    R 381.txt
7319   python           0      1      0       4345    R 419.txt
7319   python           0      1      0       4248    R 395.txt
7319   python           0      1      0       3710    R 380.txt
7319   python           0      1      0       3222    R 392.txt
7319   python           0      1      0       3173    R 396.txt
7319   python           0      1      0       3173    R 391.txt
7319   python           0      1      0       3027    R 387.txt
7319   python           0      1      0       2978    R 386.txt
7319   python           0      1      0       2880    R 403.txt
7319   python           0      1      0       2880    R 378.txt
7319   python           0      1      0       2880    R 418.txt
7319   python           0      1      0       2832    R 416.txt
7319   python           0      1      0       2783    R 417.txt
7319   python           0      1      0       2734    R 415.txt
7319   python           0      1      0       2636    R 390.txt
7319   python           0      1      0       2587    R 405.txt

02:25:27 loadavg: 4.52 3.94 2.39 2/184 7323

TID    COMM             READS  WRITES R_Kb    W_Kb    T FILE
7319   python           0      1      0       4882    R 423.txt
7319   python           0      1      0       4492    R 428.txt
7319   python           0      1      0       4492    R 431.txt
7319   python           0      1      0       3564    R 435.txt
7319   python           0      1      0       2929    R 426.txt
7319   python           0      1      0       2929    R 427.txt
7319   python           0      1      0       2880    R 429.txt
7319   python           0      1      0       2880    R 422.txt
7319   python           0      1      0       2832    R 434.txt
7319   python           0      1      0       2783    R 432.txt
7319   python           0      1      0       2587    R 430.txt
7319   python           0      1      0       2587    R 424.txt
7319   python           0      1      0       2587    R 437.txt
7319   python           0      1      0       2539    R 420.txt
7319   python           0      1      0       2539    R 425.txt
7319   python           0      1      0       2539    R 436.txt
7319   python           0      1      0       2148    R 433.txt
7319   python           0      1      0       1757    R 421.txt
7314   filetop          2      0      2       0       R loadavg

02:25:28 loadavg: 4.40 3.93 2.39 2/184 7323
```

```
root@perf:/usr/share/bcc/tools# ./opensnoop
PID    COMM               FD ERR PATH
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/156.txt
7502   opensnoop          -1   2 /usr/lib/python2.7/encodings/ascii.x86_64-linux-gnu.so
7502   opensnoop          -1   2 /usr/lib/python2.7/encodings/ascii.so
7502   opensnoop          -1   2 /usr/lib/python2.7/encodings/asciimodule.so
7502   opensnoop          12   0 /usr/lib/python2.7/encodings/ascii.py
7502   opensnoop          13   0 /usr/lib/python2.7/encodings/ascii.pyc
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/157.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/158.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/159.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/160.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/161.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/162.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/163.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/164.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/165.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/166.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/167.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/168.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/169.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/170.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/171.txt
6370   python              6   0 /tmp/122fb6f6-bd72-11e9-8218-0242ac110002/172.txt
```

opensnoop 输出结果的 /tmp 后面uuid是会变化的

