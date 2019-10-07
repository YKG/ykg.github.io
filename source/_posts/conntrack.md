---
title: conntrack
date: 2019-10-07 15:28:50
tags: [NAT, netfilter]
---

Linux中iptables设置nat时，NAT的具体实现是依靠conntrack来实现的，具体参考这个[回答][1]

安装conntrack-tools花了点功夫。

从[netfilter downloads][3]下载`conntrack-tools`,解压，按照INSTALL的说明安装

```bash
wget https://netfilter.org/projects/conntrack-tools/files/conntrack-tools-1.4.5.tar.bz2
tar xf conntrack-tools-1.4.5.tar.bz2
cd conntrack-tools-1.4.5/

# install
./configure --prefix=/usr
make
make install
```

中途会提示各种包`libnetfilter-*`缺失，或者版本过低，都是按照同样的方法下载然后再configure和make(install)，或者版本够的话，也可以apt安装

比如`libnetfilter_queue`

```bash
apt install libnetfilter-queue-dev # 一般都是 libnetfilter-*-dev这种形式
```

如果版本过低，需要将原来的lib删掉，再重新make安装，比如`libnetfilter_conntrack`在ubuntu18.04lts中安装的是1.0.6版本，但要求1.0.7的

```bash
apt uninstall libnetfilter-conntrack-dev

cd libnetfilter_conntrack-1.0.7/
# install
./configure --prefix=/usr
make
make install

# 检查版本
pkg-config --modversion libnetfilter_conntrack
#1.0.7
```


### 参考资料
- [RedHat FORWARD and NAT Rules][2]


[1]: https://superuser.com/questions/1269859/linux-netfilter-how-does-connection-tracking-track-connections-changed-by-nat
[2]: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/Security_Guide/s1-firewall-ipt-fwd.html
[3]: http://conntrack-tools.netfilter.org/downloads.html
