---
title: 腾讯云带宽测试
date: 2019-07-01 18:33:23
tags: [带宽, 腾讯云]
---

先在服务器上装了docker-ce，启动iperf3服务端：

```bash
sudo docker run  -it --rm --name=iperf3-server -p 5201:5201 networkstatic/iperf3 -s
```

本地
```bash
iperf3 -c bj
```

```
[  7] local 10.21.39.50 port 61827 connected to X.X.X.X port 5201
[ ID] Interval           Transfer     Bitrate
[  7]   0.00-1.00   sec  1.06 MBytes  8.87 Mbits/sec
[  7]   1.00-2.00   sec  1010 KBytes  8.27 Mbits/sec
[  7]   2.00-3.00   sec   939 KBytes  7.69 Mbits/sec
[  7]   3.00-4.00   sec  1.03 MBytes  8.64 Mbits/sec
[  7]   4.00-5.00   sec   919 KBytes  7.53 Mbits/sec
[  7]   5.00-6.00   sec  1.07 MBytes  8.99 Mbits/sec
[  7]   6.00-7.00   sec  1.33 MBytes  11.2 Mbits/sec
[  7]   7.00-8.00   sec  1.12 MBytes  9.39 Mbits/sec
[  7]   8.00-9.00   sec  1.16 MBytes  9.76 Mbits/sec
[  7]   9.00-10.00  sec  1.20 MBytes  10.1 Mbits/sec
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  7]   0.00-10.00  sec  10.8 MBytes  9.04 Mbits/sec                  sender
[  7]   0.00-10.00  sec  10.7 MBytes  8.95 Mbits/sec                  receiver
```


北京nas
```bash
docker run  -it --rm networkstatic/iperf3 -c bj
```

```
[  4] local 10.0.3.2 port 35418 connected to X.X.X.X port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec  15.0 MBytes   126 Mbits/sec  502   20.7 KBytes
[  4]   1.00-2.00   sec  2.50 MBytes  21.0 Mbits/sec   57   19.3 KBytes
[  4]   2.00-3.00   sec  2.50 MBytes  21.0 Mbits/sec   63   17.9 KBytes
[  4]   3.00-4.00   sec  2.50 MBytes  21.0 Mbits/sec   60   12.4 KBytes
[  4]   4.00-5.00   sec  2.50 MBytes  21.0 Mbits/sec   60   22.1 KBytes
[  4]   5.00-6.00   sec  2.50 MBytes  21.0 Mbits/sec   47   22.1 KBytes
[  4]   6.00-7.00   sec  2.50 MBytes  21.0 Mbits/sec   61   19.3 KBytes
[  4]   7.00-8.00   sec  2.50 MBytes  21.0 Mbits/sec   50   16.5 KBytes
[  4]   8.00-9.00   sec  2.50 MBytes  21.0 Mbits/sec   44   29.0 KBytes
[  4]   9.00-10.00  sec  2.50 MBytes  21.0 Mbits/sec   69   24.8 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  37.5 MBytes  31.5 Mbits/sec  1013             sender
[  4]   0.00-10.00  sec  26.3 MBytes  22.0 Mbits/sec                  receiver
```


Azure hk
```
[  4] local 172.17.0.8 port 54104 connected to X.X.X.X port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec  3.37 MBytes  28.3 Mbits/sec  216   78.6 KBytes
[  4]   1.00-2.00   sec  1.24 MBytes  10.4 Mbits/sec   57   29.0 KBytes
[  4]   2.00-3.00   sec   317 KBytes  2.60 Mbits/sec   29   5.52 KBytes
[  4]   3.00-4.00   sec   317 KBytes  2.60 Mbits/sec    8   12.4 KBytes
[  4]   4.00-5.00   sec  0.00 Bytes  0.00 bits/sec   14   8.27 KBytes
[  4]   5.00-6.00   sec   317 KBytes  2.60 Mbits/sec    4   9.65 KBytes
[  4]   6.00-7.00   sec   317 KBytes  2.60 Mbits/sec   14   11.0 KBytes
[  4]   7.00-8.00   sec   317 KBytes  2.60 Mbits/sec    9   16.5 KBytes
[  4]   8.00-9.00   sec   317 KBytes  2.60 Mbits/sec    8   8.27 KBytes
[  4]   9.00-10.00  sec   317 KBytes  2.60 Mbits/sec    6   15.2 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  6.78 MBytes  5.69 Mbits/sec  365             sender
[  4]   0.00-10.00  sec  5.99 MBytes  5.02 Mbits/sec                  receiver
```


Vultr Tokyo
```
[  4] local 172.17.0.4 port 42992 connected to X.X.X.X port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec   571 KBytes  4.68 Mbits/sec   43    145 KBytes
[  4]   1.00-2.00   sec   634 KBytes  5.19 Mbits/sec   44   71.7 KBytes
[  4]   2.00-3.00   sec   317 KBytes  2.60 Mbits/sec   43   71.7 KBytes
[  4]   3.00-4.00   sec   317 KBytes  2.60 Mbits/sec   25   34.5 KBytes
[  4]   4.00-5.00   sec  0.00 Bytes  0.00 bits/sec   38   49.6 KBytes
[  4]   5.00-6.00   sec   317 KBytes  2.60 Mbits/sec   23   71.7 KBytes
[  4]   6.00-7.00   sec   381 KBytes  3.12 Mbits/sec   43   29.0 KBytes
[  4]   7.00-8.00   sec   317 KBytes  2.60 Mbits/sec   21   35.9 KBytes
[  4]   8.00-9.00   sec   317 KBytes  2.60 Mbits/sec   31   57.9 KBytes
[  4]   9.00-10.00  sec   317 KBytes  2.60 Mbits/sec   64    119 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  3.41 MBytes  2.86 Mbits/sec  375             sender
[  4]   0.00-10.00  sec  2.69 MBytes  2.25 Mbits/sec                  receiver
```


GCE tw
```
[  4] local 172.17.0.3 port 45792 connected to X.X.X.X port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-1.00   sec  1.24 MBytes  10.4 Mbits/sec   53   31.6 KBytes
[  4]   1.00-2.00   sec   506 KBytes  4.15 Mbits/sec   19   13.8 KBytes
[  4]   2.00-3.00   sec   190 KBytes  1.55 Mbits/sec    6   6.88 KBytes
[  4]   3.00-4.00   sec   126 KBytes  1.04 Mbits/sec    5   9.62 KBytes
[  4]   4.00-5.00   sec   253 KBytes  2.07 Mbits/sec    8   9.62 KBytes
[  4]   5.00-6.00   sec   190 KBytes  1.55 Mbits/sec    5   9.62 KBytes
[  4]   6.00-7.00   sec   190 KBytes  1.55 Mbits/sec    4   6.88 KBytes
[  4]   7.00-8.00   sec  0.00 Bytes  0.00 bits/sec    3   6.88 KBytes
[  4]   8.00-9.00   sec   253 KBytes  2.07 Mbits/sec    4   8.25 KBytes
[  4]   9.00-10.00  sec   126 KBytes  1.04 Mbits/sec    5   5.50 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-10.00  sec  3.03 MBytes  2.54 Mbits/sec  112             sender
[  4]   0.00-10.00  sec  2.72 MBytes  2.29 Mbits/sec                  receiver
```

再换到其他服务器作为服务端，发现可能主要还是受到发起方带宽限制。

测试这个是考虑将nextcloud部署到nas上还是腾讯云上，感觉还是nas性能和带宽更好，先放nas了。
