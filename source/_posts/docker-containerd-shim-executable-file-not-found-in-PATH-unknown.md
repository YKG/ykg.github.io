---
title: docker-containerd-shim executable file not found in $PATH unknown
date: 2019-07-24 09:35:06
tags: [Docker]
---

在NAS上启动一个新容器的时候出现了标题的错误，搜了一下，重启docker daemon解决，原因未知

```s
systemctl restart docker
```
