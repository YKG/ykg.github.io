---
title: Linux性能优化-实验报告06
date: 2019-08-03 17:27:01
tags: [Perf]
---

```
docker run --name nginx -p 10000:80 -itd feisky/nginx:sp
docker run --name phpfpm -itd --network container:nginx feisky/php-fpm:sp
```

perf report结果

![](perf.png)

但是如果perf record的时间长了以后，反而没有这种结果，根本找不到stress。时间短的可以。

execsnoop执行没有结果
