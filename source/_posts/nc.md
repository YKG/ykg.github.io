---
title: nc
date: 2019-07-24 16:44:55
tags: [Linux]
---

nc: NetCat

最基础C/S例子：

```bash
# server

nc -l 9999
```

```bash
# client

nc server 9999
```

然后client/server上都可以进行输入，并且会被传递到对方的控制台，这样对于临时复制一些东西很方便，测试连通行也很好，nc是默认安装的（至少在Ubuntu 18.04和Mac上）。另外文件传入的例子可以看man nc的例子。
