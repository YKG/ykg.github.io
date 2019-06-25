---
title: 怎样获取QNAP的docker container中的bash
date: 2019-06-25 22:00:52
tags: [QNAP, docker]
---

在云服务器的Ubuntu中，可以直接使用
```bash
docker exec saitu-db bash
```

但在QNAP中得加上`--it`参数

```bash
docker exec -it saitu-db bash
```
