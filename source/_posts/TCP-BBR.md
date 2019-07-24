---
title: TCP BBR
date: 2019-07-24 11:17:34
tags: [Linux, Network]
---

很早之前使用ss的时候就看到有提到bbr加速，之后也看过YouTube上bbr的作者们的演示视频，因为这几天在弄VPN，看到用speedtest网站测下来的结果表明，ss的带宽不但比wireguard高个1～2MBps，而且稳，全程都在20～21Mbps，相比之下WireGuard是缓慢上升到19Mbps的样子，同样上传带宽ss也要比wg高上2～3Mbps的样子。然后想到wg用udp没办法，是不是可以再对ss进行bbr优化，查了下bbr，现在18.04的ubuntu默认tcp拥塞控制算法还是cubic，据说19.04默认为bbr了，也就没折腾，又看了看Wikipedia上相关词条，英文的质量比中文的高太多，其中[TCP congestion control][1]里面介绍了很多种算法，还有2019年有人提出的Elastic-TCP，虽然没细看，但觉得以后可以花点时间了解一下各种拥塞控制算法


[1]: https://en.wikipedia.org/wiki/TCP_congestion_control
