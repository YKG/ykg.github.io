---
title: WireGuard全局代理
date: 2019-10-07 09:15:37
tags: [WireGuard, VPN, iptables]
---

知道WireGuard可以代理全局，需要在`client.conf`中指明

```conf
AllownIPs = 0.0.0.0/0 # 不考虑IPv6
```

在服务器端使用`tcpdump -i tun1`可以看到来自client的请求包，但都没有回应。这个问题困扰了好久，后来发现可以和server的LAN IP通信，于是才有了通过WireGuard创建的局域网来访问服务器端的Shadowsocks服务，这样就规避了上面的问题。

昨天夜里睡了之后，想之前看WireGuard的论文，说服务端也会检查AllowIPs来决定是否要发送出去，以为把服务端Client Peer的AllownIPs也改成`0.0.0.0/0`就能通，其实也有顾虑，因为从之前的经验来看，显然wg会根据这个地址来决定给谁发去，如果多于一个client，有人是4个0，那是不是全都发给4个0的，其他的没有？抑或其他的也发，一个包像局域网一样给每个匹配的复制一份发出去？

```conf
[Peer] # Client
# AllownIPs = 192.168.2.3/32 # 原配置
AllownIPs = 0.0.0.0/0        # 以为用这个就可以路由全部流量
```

事实证明还是不工作。在弄了一会儿iptables之后，觉得说得通的结果证明确实是有效的：

```
iptables -A FORWARD -i tun1 -o eth0 -j ACCEPT
iptables -A FORWARD -i eth0 -o tun1 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o eth0 -j MASQUERADE
```

再看wg的`PostUp`和`PostDown`就更清楚了

```
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```
