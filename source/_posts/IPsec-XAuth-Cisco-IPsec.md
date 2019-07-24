---
title: IPsec/XAuth (Cisco IPsec)
date: 2019-07-24 09:25:50
tags: [VPN]
---

看这个[文档][1]了解IPsec/XAuth和Cisco IPsec是一个东西。

因为Mac上的VPN可以直接使用Cisco IPSec类型的，Windows下没有，文档里介绍了Shrew Soft客户端，经尝试确实可以用，虽然大部分时候是连不上的，偶尔能连接成功，成功就可以用。但这个确实很不稳定，后来尝试修改500和4500两个端口到50000+以上的端口，感觉连通成功比例大大提高，极少连接失败，配合着减小MTU似乎也有影响，但具体影响效果没有充分验证。可是Mac上是无法配置这些端口的。

另外尝试想使用nginx的upstream配置（类似自己配的ss中继），代理udp中继，但并没有工作。

[1]: https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-xauth-zh.md
