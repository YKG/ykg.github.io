---
title: Linux性能优化-实验报告17
date: 2019-08-05 05:38:48
tags: [Perf]
---

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4052245BD4284CDD
echo "deb https://repo.iovisor.org/apt/xenial xenial main" | sudo tee /etc/apt/sources.list.d/iovisor.list
sudo apt-get update
sudo apt-get install -y bcc-tools libbcc-examples linux-headers-$(uname -r)
```

```
export PATH=$PATH:/usr/share/bcc/tools
```

```
# install go
$ wget https://dl.google.com/go/go1.12.7.linux-amd64.tar.gz
$ tar -C /usr/local -xzf go1.12.7.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
$ export GOPATH=~/go
$ export PATH=~/go/bin:$PATH
$ go get golang.org/x/sys/unix
$ go get github.com/tobert/pcstat/pcstat
```

```
root@perf:/home/ykg# pcstat /bin/ls
+---------+----------------+------------+-----------+---------+
| Name    | Size (bytes)   | Pages      | Cached    | Percent |
|---------+----------------+------------+-----------+---------|
| /bin/ls | 133792         | 33         | 33        | 100.000 |
+---------+----------------+------------+-----------+---------+
```

```
root@perf:/home/ykg# dd if=/dev/sda1 of=file bs=1M count=512
512+0 records in
512+0 records out
536870912 bytes (537 MB, 512 MiB) copied, 19.0035 s, 28.3 MB/s
root@perf:/home/ykg# echo 3 > /proc/sys/vm/drop_caches
root@perf:/home/ykg# pcstat file
+-------+----------------+------------+-----------+---------+
| Name  | Size (bytes)   | Pages      | Cached    | Percent |
|-------+----------------+------------+-----------+---------|
| file  | 536870912      | 131072     | 42496     | 032.422 |
+-------+----------------+------------+-----------+---------+
```

```
root@perf:/home/ykg# dd if=file of=/dev/null bs=1M
512+0 records in
512+0 records out
536870912 bytes (537 MB, 512 MiB) copied, 12.9729 s, 41.4 MB/s
root@perf:/home/ykg# dd if=file of=/dev/null bs=1M
512+0 records in
512+0 records out
536870912 bytes (537 MB, 512 MiB) copied, 0.146201 s, 3.7 GB/s
```

```
root@perf:/home/ykg# pcstat file
+-------+----------------+------------+-----------+---------+
| Name  | Size (bytes)   | Pages      | Cached    | Percent |
|-------+----------------+------------+-----------+---------|
| file  | 536870912      | 131072     | 131072    | 100.000 |
+-------+----------------+------------+-----------+---------+
```

# EXP2

```
docker run --privileged --name=app -itd feisky/app:io-direct

root@perf:/home/ykg# docker logs app
Reading data from disk /dev/sdb1 with buffer size 33554432
Time used: 1.415684 s to read 33554432 bytes
Time used: 1.441454 s to read 33554432 bytes
Time used: 1.449582 s to read 33554432 bytes
Time used: 1.449696 s to read 33554432 bytes
Time used: 1.449216 s to read 33554432 bytes
root@perf:/home/ykg#
```

```
22:05:00 Buffers MB: 13 / Cached MB: 760 / Sort: HITS / Order: ascending
PID      UID      CMD              HITS     MISSES   DIRTIES  READ_HIT%  WRITE_HIT%
    1121 root     atopacctd               1        0        0     100.0%       0.0%
    1504 root     python3                 2        0        0     100.0%       0.0%
    4432 root     dockerd                 8        0        2      75.0%       0.0%
    4303 root     cachetop               14        0        0     100.0%       0.0%
    1442 root     python3                23        2        1      88.0%       4.0%
    4701 root     sh                    110        0        0     100.0%       0.0%
    4703 root     sh                    114        0        0     100.0%       0.0%
    4700 root     python3               366        0        0     100.0%       0.0%
    4702 root     python3               366        0        0     100.0%       0.0%
    4702 root     sh                    401        0        0     100.0%       0.0%
    4700 root     sh                    425        0        0     100.0%       0.0%
    4701 root     iptables              441        0        0     100.0%       0.0%
    4703 root     iptables              522        0        0     100.0%       0.0%
    4611 root     app                  1024        0        0     100.0%       0.0%
```

```
root@perf:/home/ykg# strace -p $(pgrep app)
strace: Process 4611 attached
write(1, "Time used: 1.450181 s to read 33"..., 45) = 45
close(4)                                = 0
munmap(0x7f46451db000, 33558528)        = 0
nanosleep({tv_sec=1, tv_nsec=0}, 0x7ffed794fdb0) = 0
openat(AT_FDCWD, "/dev/sdb1", O_RDONLY|O_DIRECT) = 4
mmap(NULL, 33558528, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f46451db000
read(4, "\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"..., 33554432) = 33554432
write(1, "Time used: 1.447891 s to read 33"..., 45) = 45
close(4)                                = 0
munmap(0x7f46451db000, 33558528)        = 0
nanosleep({tv_sec=1, tv_nsec=0}, 0x7ffed794fdb0) = 0
openat(AT_FDCWD, "/dev/sdb1", O_RDONLY|O_DIRECT) = 4
mmap(NULL, 33558528, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f46451db000
read(4, "\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"..., 33554432) = 33554432
write(1, "Time used: 1.448310 s to read 33"..., 45) = 45
close(4)                                = 0
```


```
# 删除上述案例应用
$ docker rm -f app

# 运行修复后的应用
$ docker run --privileged --name=app -itd feisky/app:io-cached

root@perf:/home/ykg# docker logs app
Reading data from disk /dev/sdb1 with buffer size 33554432
Time used: 1.396874 s to read 33554432 bytes
Time used: 0.018585 s to read 33554432 bytes
Time used: 0.017417 s to read 33554432 bytes
Time used: 0.016598 s to read 33554432 bytes
Time used: 0.017005 s to read 33554432 bytes
Time used: 0.016940 s to read 33554432 bytes
Time used: 0.016698 s to read 33554432 bytes
```

```
22:08:15 Buffers MB: 46 / Cached MB: 772 / Sort: HITS / Order: ascending
PID      UID      CMD              HITS     MISSES   DIRTIES  READ_HIT%  WRITE_HIT%
    1121 root     atopacctd               1        0        0     100.0%       0.0%
    1504 root     python3                 2        0        0     100.0%       0.0%
    4362 root     dockerd                 4        0        1      75.0%       0.0%
    4404 root     dockerd                 4        0        1      75.0%       0.0%
    4433 root     dockerd                12        0        3      75.0%       0.0%
    4303 root     cachetop               15        0        0     100.0%       0.0%
     376 root     jbd2/sda1-8            17       13       11      20.0%       6.7%
    1442 root     python3                47        4        2      88.2%       3.9%
    5079 root     sh                    110        0        0     100.0%       0.0%
    5081 root     sh                    114        0        0     100.0%       0.0%
    5078 root     python3               366        0        0     100.0%       0.0%
    5080 root     python3               366        0        0     100.0%       0.0%
    5080 root     sh                    389        0        0     100.0%       0.0%
    5078 root     sh                    410        0        0     100.0%       0.0%
    5079 root     iptables              442        0        0     100.0%       0.0%
    5081 root     iptables              521        0        0     100.0%       0.0%
    4988 root     app                 40960        0        0     100.0%       0.0%
```
