---
title: sshd端口设置
date: 2019-07-03 10:55:44
tags: [ssh]
---

```bash
# Ubuntu 18.04
sudo vi /etc/ssh/sshd_config

#Port 22
#Port 12345

sudo systemctl restart sshd
```

如有需要还要设置服务器端防火墙放行设置的端口

客户端配置

```conf
# ~/.ssh/config

Host abc
    Hostname 1.2.3.4
    Port 12345
    User ubuntu
```
