---
title: GatewayPorts
date: 2019-07-14 19:38:29
tags: [ssh]
---

今天为了将本地服务进行内网穿透供外网使用，试了`ssh -R`，但`ss`结果看到的只是绑定了`127.0.0.1`的接口，即使指定了`0.0.0.0`前缀也不行，查了下，是要在`/etc/ssh/sshd_config`的配置里把`GatewayPorts`设置为`yes`

```bash
# GatewayPorts no 的情况
local$ ssh -R 0.0.0.0:2080:localhost:8080 vps
vps$ ss -ltn
# State         Local Address:Port
# LISTEN        127.0.0.1:2080

# GatewayPorts yes
local$ ssh -R 0.0.0.0:2080:localhost:8080 vps
vps$ ss -ltn
# State         Local Address:Port
# LISTEN        0.0.0.0:2080
```
