---
title: systemctl VS sysctl VS service VS init.d
date: 2019-07-24 09:51:32
tags:
---

不了解这些东西，平时用到就瞎试。这次了解记录一下。


### systemctl

说是systemd的管理工具，systemd复制管理系统和进程，systemd是pid=1的进程。

```bash
systemctl
systemctl status

# check pid = 1
# ps -q 1
```

### service

说service是一个用来start/stop/reload各种服务的工具，是一个wrapper，不同Linux发行版可能使用不同的init系统管理工具，service可以统一起来。所以service算是systemctl的上层封装。

```bash
service --status-all
service mysql status
```

### sysctl

FreeBSD上给出的介绍是：

> sysctl(8) 是一个允许您改变正在运行中的 FreeBSD 系统的接口。它包含一些 TCP/IP 堆栈和虚拟内存系统的高级选项， 这可以让有经验的管理员提高引人注目的系统性能。用 sysctl(8) 可以读取设置超过五百个系统变量。

linuxde上看到的是：

> sysctl命令被用于在内核运行时动态地修改内核的运行参数，可用的内核参数在目录/proc/sys中。它包含一些TCP/ip堆栈和虚拟内存系统的高级选项， 这可以让有经验的管理员提高引人注目的系统性能。用sysctl可以读取设置超过五百个系统变量。

```
sysctl -w net.ipv4.ip_forward=1

sysctl kern.maxfiles=5000
```

### init.d

[这里][1]有个例子，说

A systemd OS will use something like this for restarts:

```
systemctl restart httpd.service
```

where the script lives at:
```
/etc/systemd/system/httpd.service
```

while the traditional init.d uses a more direct script restart with:
```
/etc/init.d/httpd restart
```

另外，[鸟哥的Linux私房菜][2]讲了像`/etc/init.d/`这种风格的东西就是古老的SystemV方式，现在都早已经systemd提到systemV。

[1]: https://help.directadmin.com/item.php?id=642
[2]: http://linux.vbird.org/linux_basic/0560daemons.php
