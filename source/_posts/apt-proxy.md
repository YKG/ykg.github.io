---
title: apt proxy
date: 2020-02-03 00:47:29
tags: [apt, proxy]
---

安装linux-mptcp太慢，使用ALL_PROXY=socks5://…前缀无效。

最后才知道原来apt不会用那个参数，发现了[这篇文章][1]介绍的`-o Acquire::http::proxy`参数

```bash
sudo apt-get -o Acquire::http::proxy="socks5h://127.0.0.1:1080/" update
```

这个才行。

同理修改的结果：

```bash
sudo apt-get -o Acquire::http::proxy="socks5h://127.0.0.1:11080/" install linux-mptcp
```

此外看到云风blog也有mptcp的描述，好几年前了，估计我之前也是看过的，完全没有印象了。


#### 2020-02-05 14：40补记

上面的-o参数实在腾讯云的服务器上运行的，ok。
但刚刚我在自己本机的ubuntu虚拟机里运行，一直不行。最后通过创建一个apt配置文件解决的。不知道为什么放在命令行里无效。

```conf
# /etc/apt/apt.conf.d/proxy.conf

Acquire::http::proxy="socks5h://127.0.0.1:11080/";  # 后面要加分号
```

仅使用这一行就够。虽然我看下载链接是https的。


[1]: https://www.jianshu.com/p/bc4d7b758503
