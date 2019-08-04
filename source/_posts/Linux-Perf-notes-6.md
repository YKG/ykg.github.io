---
title: Linux性能优化-实验报告10
date: 2019-08-04 13:46:17
tags: [Perf]
---

```
# 运行 Nginx 服务并对外开放 80 端口
$ docker run -itd --name=nginx -p 80:80 nginx
```

```
# -S 参数表示设置 TCP 协议的 SYN（同步序列号），-p 表示目的端口为 80
# -i u100 表示每隔 100 微秒发送一个网络帧
# 注：如果你在实践过程中现象不明显，可以尝试把 100 调小，比如调成 10 甚至 1
$ hping3 -S -p 80 -i u100 52.163.188.200
```

```
$ sudo hping3 -S -p 80 -i u100 52.163.188.200
...
len=44 ip=52.163.188.200 ttl=63 DF id=0 sport=80 flags=SA seq=0 win=29200 rtt=0.0 ms
len=44 ip=52.163.188.200 ttl=63 DF id=0 sport=80 flags=SA seq=0 win=29200 rtt=0.0 ms
len=44 ip=52.163.188.200 ttl=63 DF id=0 sport=80 flags=SA seq=0 win=29200 rtt=0.0 ms
[send_ip] sendto: Operation not permitted

$ sudo su root
# sudo hping3 -S -p 80 -i u100 52.163.188.200
...
[send_ip] sendto: Operation not permitted
```

看了下应该是azure不支持icmp的原因，切换到gcp上。顺便看了一眼atop，gcp机器avio是小于1ms的

在gcp上测了，降低到10us间隔，依然很流畅。。降到1us 100% loss，估计会被gcp平台过滤掉的吧。打算回到家里在局域网实际机器上测一下



