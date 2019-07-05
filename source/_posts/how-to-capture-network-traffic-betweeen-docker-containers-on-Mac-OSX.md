---
title: 怎样使用wireshark捕获Mac OSX上docker container之间的流量
date: 2019-07-05 19:17:03
tags: [Wireshark]
---

目前似乎没有办法，或许可以使用iptables的`TEE`将流量tee到本地某个端口

当前做法仍然是使用tcpdump，结合mutagen将`.pcap`文件和本地目录自动同步，面得手动复制了，Wireshark可以直接打开本地的`.pcap`来分析

例子：

```bash
docker-compose exec app sh

apk add tcpdump

tcpdump # 监控所有
tcpdump -i eth0 port 80
```
