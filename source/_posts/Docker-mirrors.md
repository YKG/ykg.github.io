---
title: Docker镜像
date: 2020-02-09 17:41:16
tags: [Docker, Mirror]
---


Docker Desktop for Mac 设置 registry-mirrors 为

```
http://dockerhub.azk8s.cn
```

可以大大提到下载速度。


不能使用https，至少目前版本的app有问题，对于https开头的处理不对，会提示类似 

```
No certs for egitstry.docker.com
```

即会把 `://` 后的第一个字母吞掉，还工作不正常
