---
title: use Cisco IPSec instead of L2TP
date: 2019-07-01 10:42:56
tags: [VPN]
---

使用[这个指南][1]搭建了jp和hk节点的VPN，直接使用L2TP无法连接，切换至`Cisco IPSec`工作正常。

L2TP连接国内的机器则是正常的，比如连接在北京NAS的VPN就是L2TP，很流畅。

仅就Youtube的播放速率来看，还是走NAS代理到hk是最流畅的，直接连的不管是VPN还是ss都还是差一些。


[1]: https://github.com/hwdsl2/docker-ipsec-vpn-server/blob/master/README-zh.md
