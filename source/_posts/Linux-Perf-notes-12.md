---
title: Linux性能优化-实验报告26
date: 2019-08-06 18:55:32
tags: [Perf]
---

```
docker run -v /tmp:/tmp --name=app -itd feisky/logapp 
```

```
root@perf:/home/ykg# top
top - 10:59:38 up  9:22,  1 user,  load average: 3.54, 1.66, 0.64
Tasks: 130 total,   1 running,  70 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.0 us,  7.4 sy,  0.0 ni,  0.3 id, 92.3 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu1  :  0.0 us,  0.3 sy,  0.0 ni,  2.0 id, 97.7 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8143064 total,  4595940 free,  1336872 used,  2210252 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  6490488 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 37414 root      20   0  963312 934312   5292 D   8.0 11.5   0:19.82 python
 35141 root      20   0       0      0      0 D   0.7  0.0   0:00.58 kworker/u256:2+
 36735 root      20   0       0      0      0 I   0.3  0.0   0:00.05 kworker/u256:0-
     1 root      20   0   78044   9124   6568 S   0.0  0.1   0:04.46 systemd
```

```
root@perf:/home/ykg# iostat -x -d 1
Linux 4.18.0-1025-azure (perf) 	08/06/19 	_x86_64_	(2 CPU)

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     1.60     0.00   0.00   0.00
sdb              0.02    0.03      0.48    506.03     0.00     0.52   0.57  95.30    0.40  240.31   0.07    22.98 19644.38 1328.16   6.17
sda              5.29    1.37    629.18    285.39     0.05     1.12   0.89  44.97   10.90  481.50   1.69   118.92   207.69 148.83  99.20

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00   53.00      0.00  27136.00     0.00     0.00   0.00   0.00    0.00 2968.98  67.29     0.00   512.00  18.34  97.20

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              1.00   44.00      4.00  22020.00     0.00     0.00   0.00   0.00 4236.00 3488.82  16.84     4.00   500.45  22.22 100.00

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00   56.00      0.00  28672.00     0.00     0.00   0.00   0.00    0.00  302.43 118.77     0.00   512.00  17.36  97.20

...
Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00   56.00      0.00  27188.00     0.00     0.00   0.00   0.00    0.00  587.71 162.86     0.00   485.50  17.36  97.20
```


```
root@perf:/home/ykg# pidstat -d 1
Linux 4.18.0-1025-azure (perf) 	08/06/19 	_x86_64_	(2 CPU)

11:02:18      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:19        0     37414      0.00      0.00      0.00     110  python

11:02:19      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:20        0     37414      0.00      0.00      0.00      95  python

11:02:20      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:21        0     37414      0.00      0.00      0.00     100  python

11:02:21      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:22        0     37414      0.00      0.00      0.00      99  python

11:02:22      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:23        0     37414      0.00 283972.00      0.00      54  python
11:02:23        0     37537      0.00      0.00      0.00     627  kworker/u256:3-events_power_efficient

11:02:23      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:24        0     37414      0.00  23232.00      0.00      34  python

11:02:24      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:25        0     37414      0.00  93156.00      0.00      91  python

11:02:25      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:26        0     37414      0.00  52788.00      0.00      95  python

11:02:26      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:27        0     37414      0.00  52788.00      0.00      94  python

11:02:27      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:28        0     37414      0.00  35828.00      0.00      97  python

11:02:28      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:29        0     37414      0.00  27348.00      0.00      98  python

11:02:29      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:30        0     37414      0.00  27772.00      0.00      98  python

11:02:30      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:31        0     37414      0.00  17520.00      0.00      63  python

11:02:31      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command
11:02:32        0       375      0.00      0.00      0.00    2846  jbd2/sda1-8
11:02:32        0       454      0.00      4.00      0.00       0  systemd-journal
11:02:32        0      1476      0.00      4.00      0.00    5634  python3
11:02:32        0     37414      0.00  43960.00      0.00      64  python
```

```
root@perf:/home/ykg# strace -p 37414
strace: Process 37414 attached
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a7a0ec000
write(3, "2019-08-06 11:04:21,477 - __main"..., 314572844) = 314572844
munmap(0x7f1a7a0ec000, 314576896)       = 0
write(3, "\n", 1)                       = 1
munmap(0x7f1a8cced000, 314576896)       = 0
select(0, NULL, NULL, NULL, {tv_sec=0, tv_usec=100000}) = 0 (Timeout)
getpid()                                = 1
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 393220096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a755ec000
mremap(0x7f1a755ec000, 393220096, 314576896, MREMAP_MAYMOVE) = 0x7f1a755ec000
munmap(0x7f1a8cced000, 314576896)       = 0
lseek(3, 0, SEEK_END)                   = 314572845
lseek(3, 0, SEEK_CUR)                   = 314572845
munmap(0x7f1a755ec000, 314576896)       = 0
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a7a0ec000
write(3, "2019-08-06 11:04:35,681 - __main"..., 314572844) = 314572844
munmap(0x7f1a7a0ec000, 314576896)       = 0
write(3, "\n", 1)                       = 1
munmap(0x7f1a8cced000, 314576896)       = 0
select(0, NULL, NULL, NULL, {tv_sec=0, tv_usec=100000}) = 0 (Timeout)
getpid()                                = 1
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 393220096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a755ec000
mremap(0x7f1a755ec000, 393220096, 314576896, MREMAP_MAYMOVE) = 0x7f1a755ec000
munmap(0x7f1a8cced000, 314576896)       = 0
lseek(3, 0, SEEK_END)                   = 629145690
lseek(3, 0, SEEK_CUR)                   = 629145690
munmap(0x7f1a755ec000, 314576896)       = 0
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a7a0ec000
write(3, "2019-08-06 11:04:44,141 - __main"..., 314572844) = 314572844
munmap(0x7f1a7a0ec000, 314576896)       = 0
write(3, "\n", 1)                       = 1
munmap(0x7f1a8cced000, 314576896)       = 0
select(0, NULL, NULL, NULL, {tv_sec=0, tv_usec=100000}) = 0 (Timeout)
getpid()                                = 1
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 393220096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a755ec000
mremap(0x7f1a755ec000, 393220096, 314576896, MREMAP_MAYMOVE) = 0x7f1a755ec000
munmap(0x7f1a8cced000, 314576896)       = 0
lseek(3, 0, SEEK_END)                   = 943718535
lseek(3, 0, SEEK_CUR)                   = 943718535
munmap(0x7f1a755ec000, 314576896)       = 0
close(3)                                = 0
stat("/tmp/logtest.txt.1", {st_mode=S_IFREG|0644, st_size=943718535, ...}) = 0
unlink("/tmp/logtest.txt.1")            = 0
stat("/tmp/logtest.txt", {st_mode=S_IFREG|0644, st_size=943718535, ...}) = 0
rename("/tmp/logtest.txt", "/tmp/logtest.txt.1") = 0
open("/tmp/logtest.txt", O_WRONLY|O_CREAT|O_APPEND|O_CLOEXEC, 0666) = 3
fcntl(3, F_SETFD, FD_CLOEXEC)           = 0
fstat(3, {st_mode=S_IFREG|0644, st_size=0, ...}) = 0
lseek(3, 0, SEEK_END)                   = 0
ioctl(3, TIOCGWINSZ, 0x7ffd7146c510)    = -1 ENOTTY (Inappropriate ioctl for device)
lseek(3, 0, SEEK_CUR)                   = 0
ioctl(3, TIOCGWINSZ, 0x7ffd7146c430)    = -1 ENOTTY (Inappropriate ioctl for device)
lseek(3, 0, SEEK_CUR)                   = 0
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a8cced000
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f1a7a0ec000
write(3, "2019-08-06 11:05:03,436 - __main"..., 314572844) = 314572844
munmap(0x7f1a7a0ec000, 314576896)       = 0
write(3, "\n", 1)                       = 1
munmap(0x7f1a8cced000, 314576896)       = 0
select(0, NULL, NULL, NULL, {tv_sec=0, tv_usec=100000}) = 0 (Timeout)
```


```
root@perf:/home/ykg# lsof -p 37414
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
python  37414 root  cwd    DIR   0,51     4096 1055849 /
python  37414 root  rtd    DIR   0,51     4096 1055849 /
python  37414 root  txt    REG   0,51    28016 1810261 /usr/local/bin/python3.7
python  37414 root  mem    REG    8,1          1810261 /usr/local/bin/python3.7 (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810990 /usr/local/lib/python3.7/lib-dynload/_queue.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810988 /usr/local/lib/python3.7/lib-dynload/_pickle.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810999 /usr/local/lib/python3.7/lib-dynload/_struct.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1811021 /usr/local/lib/python3.7/lib-dynload/select.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810996 /usr/local/lib/python3.7/lib-dynload/_socket.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810991 /usr/local/lib/python3.7/lib-dynload/_random.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810958 /usr/local/lib/python3.7/lib-dynload/_bisect.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810994 /usr/local/lib/python3.7/lib-dynload/_sha3.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810959 /usr/local/lib/python3.7/lib-dynload/_blake2.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1809588 /lib/libcrypto.so.43.0.1 (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810979 /usr/local/lib/python3.7/lib-dynload/_hashlib.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1811013 /usr/local/lib/python3.7/lib-dynload/math.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1810980 /usr/local/lib/python3.7/lib-dynload/_heapq.cpython-37m-x86_64-linux-gnu.so (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1809530 /etc/localtime (path inode=29048)
python  37414 root  mem    REG    8,1          1810370 /usr/local/lib/libpython3.7m.so.1.0 (stat: No such file or directory)
python  37414 root  mem    REG    8,1          1809585 /lib/ld-musl-x86_64.so.1 (stat: No such file or directory)
python  37414 root    0u   CHR  136,0      0t0       3 /dev/pts/0
python  37414 root    1u   CHR  136,0      0t0       3 /dev/pts/0
python  37414 root    2u   CHR  136,0      0t0       3 /dev/pts/0
python  37414 root    3w   REG    8,1        0   12705 /tmp/logtest.txt
```

```
kill -SIGUSR2 37414
```

```
top - 11:11:43 up  9:34,  1 user,  load average: 2.24, 3.88, 2.68
Tasks: 132 total,   1 running,  69 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.0 sy,  0.0 ni, 99.7 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8143064 total,  4880140 free,   703472 used,  2559452 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  7119944 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 37414 root      20   0  348904 319924   5292 S   0.3  3.9   1:04.16 python
     1 root      20   0   78044   9124   6568 S   0.0  0.1   0:04.51 systemd
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd
```

```
root@perf:/home/ykg# iostat -d -x 1
Linux 4.18.0-1025-azure (perf) 	08/06/19 	_x86_64_	(2 CPU)

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     1.60     0.00   0.00   0.00
sdb              0.02    0.03      0.47    495.21     0.00     0.51   0.57  95.30    0.40  240.31   0.07    22.98 19644.38 1328.16   6.04
sda              5.19    2.16    615.94   1135.24     0.05     1.11   0.89  34.03   12.95 4509.62  10.75   118.75   525.59 135.03  99.20

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00    4.00      0.00     72.00     0.00     4.00   0.00  50.00    0.00    1.00   0.40     0.00    18.00  99.00  39.60

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
sda              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
```

