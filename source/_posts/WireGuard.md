---
title: WireGuard
date: 2019-07-24 09:13:42
tags: [WireGuard, VPN]
---

从hwdsl2/setup-ipsec-vpn的[Readme][1]中的另见里看到了[Streisand][2]，看了Streisand中大量的VPN软件，浏览了一下，WireGuard进入视线。从22号开始折腾了两天。

Arch Linux的Wiki中[WireGuard][3]文档是很好的参考，使用最后的config文件，很容易在server上使用。

服务端例：

```s
sudo sysctl net.ipv4.ip_forward=1
```

```conf
# /etc/wireguard/wg0.conf
[Interface]
Address = 10.200.200.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = UguPyBThx/+xMXeTbRYkKlP0Wh/QZT3vTLPOVaaXTD8=

# note - substitute eth0 in the following lines to match the Internet-facing interface
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# 中文Wiki里使用的是这个
# PostUp   = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
# client foo
PublicKey = 9jalV3EEBnVXahro0pRMQ+cHlmjE33Slo9tddzCVtCw=
AllowedIPs = 10.200.200.2/32
```

```s
sudo wg-quick up wg0

#sudo wg-quick down wg0
```

客户端例子：

```conf
[Interface]
Address = 10.200.200.2/32
PrivateKey = abciengwngwedeee+cHlmjE33Slo9tddzCVtCw=
# DNS = 10.200.200.1 #需要.1提供非仅127.0.0.1:53的dns服务
# MTU = 1360 # for Google/GCP，否则Google的服务（包括YouTube）是打不开的，其他网站可以

[Peer]
PublicKey = ServerPublicKey/+xMXeTbRYkKlP0Wh/QZT3vTLPOVaaXTD8=
AllowedIPs = 0.0.0.0/0 # LAN: 10.200.200.0/24
Endpoint = serverIPAddress:51820
```

测试了下带宽，白天略低于Shadowsocks，大概是udp流量会被中间节点限制或丢弃，好像之前买的Wireshark书中提到了这个。昨天晚上对于hk的一台服务器，mac上连通是ok的，但win下不行，至今原因未知，单独[写一篇][4]诊断情况。

[1]: https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/README-zh.md#%E5%8F%A6%E8%A7%81
[2]: https://github.com/StreisandEffect/streisand
[3]: https://wiki.archlinux.org/index.php/WireGuard
[4]: https://kaige.org/2019/07/18/WireGuard-Troubleshooting
