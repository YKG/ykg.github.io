---
title: WireGuard-Troubleshooting
date: 2019-07-24 10:49:43
tags: [WireGuard, Windows]
---

问题似乎都在Windows上出现的。

### Windows上无法连通

尝试关闭公共网络的防火墙就解决了


### Handshake成功，Ping失败

昨天晚上hk的一台服务器，mac上使用正常，但win下不行，但win是连通的，但互相ping不通。抓包做了几个对比，最后在服务器端抓包(使用`tcpdump -i wg0`)，得到的情况是：
来自的Handshake是正常的，但来自Win的ping，只有请求，没有响应。但来自Mac的ping是有请求也有响应。

不知道为什么没有对于win的ping没有响应，对比win和mac的ping，包大小不一样，ttl也不一样，mac的带有timestamp，但感觉这些似乎都无关紧要。留待解决。

附注：
Wireshark真好，WireGuard的包也支持，`tcpdump -i eth0`的时候可以看到Protocol为WireGuard的包。（不过写刚才这句的时候，突然想到，Wireshark既然可以监测到是WireGuard，那么GFW岂不是也可以。。）