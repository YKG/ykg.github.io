---
title: iptables
date: 2019-07-24 10:39:13
tags: [Network, Linux]
---

先解释WireGuard使用的iptables配置

```bash
iptables -A FORWARD -i %i -j ACCEPT
iptables -A FORWARD -o %i -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

开始以为`-i/o %i`后面的`%i`是iptables的用法，查了下没找到资料，后来看`wg-quick`命令的[说明][1]得知是wg-quick的一个约定写法，`%i`用于指代`INTERFACE`，而这个`INTERFACE`即为`/etc/wireguard/INTERFACE.conf`中得到这个`INTERFACE`，所以假如使用的是`/etc/wiregurad/wg0.conf`上面也就会被解释成这样：

```bash
# filter 表 FORWARD 链 从 wg0 接口进来的包 都接受
iptables -A FORWARD -i wg0 -j ACCEPT

# filter 表 FORWARD 链 将从 wg0 接口送出的包 都允许
iptables -A FORWARD -o wg0 -j ACCEPT

# nat 表 POSTROUTING 链 伪装成公网IP从eth0接口送出
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```


读了一些有关文章，得知以下事实：

- 常用的只有`filter`和`nat`表
- 不指定`-t TABLE`的情况下，默认是`-t filter`表
- 简化的(常用的)iptables图

```
                             XXXXXXXXXXXXXXXXXX
                             XXX     Network    XXX
                               XXXXXXXXXXXXXXXXXX
                                       +
                                       |
                                       v
 +-------------+              +------------------+
 |table: filter| <---+        | table: nat       |
 |chain: INPUT |     |        | chain: PREROUTING|
 +-----+-------+     |        +--------+---------+
       |             |                 |
       v             |                 v
 [local process]     |           ****************          +--------------+
       |             +---------+ Routing decision +------> |table: filter |
       v                         ****************          |chain: FORWARD|
****************                                           +------+-------+
Routing decision                                                  |
****************                                                  |
       |                                                          |
       v                        ****************                  |
+-------------+       +------>  Routing decision  <---------------+
|table: nat   |       |         ****************
|chain: OUTPUT|       |               +
+-----+-------+       |               |
      |               |               v
      v               |      +-------------------+
+--------------+      |      | table: nat        |
|table: filter | +----+      | chain: POSTROUTING|
|chain: OUTPUT |             +--------+----------+
+--------------+                      |
                                      v
                               XXXXXXXXXXXXXXXXXX
                             XXX    Network     XXX
                               XXXXXXXXXXXXXXXXXX
```


参考：
[iptables详解（1）：iptables概念](https://www.zsythink.net/archives/1199) (第二张图非常好！)
[iptables - Arch Linux Wiki](https://wiki.archlinux.org/index.php/Iptables)
[7.4. FORWARD and NAT Rules - RedHat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/Security_Guide/s1-firewall-ipt-fwd.html)
[What is MASQUERADE in the context of iptables? - askubuntu](https://askubuntu.com/questions/466445/what-is-masquerade-in-the-context-of-iptables)

[1]: https://git.zx2c4.com/WireGuard/about/src/tools/man/wg-quick.8
