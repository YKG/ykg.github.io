---
title: 服务器监控
date: 2019-09-15 13:15:18
tags: [监控]
---

使用了DataDog处理nginx的访问日志，但还有个问题就是access.log文件datadog-agent没有权限访问，每次只能对/var/log/nginx目录下的所有文件进行`chmod o+r`才行

