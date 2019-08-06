---
title: apt镜像
date: 2019-08-06 09:14:01
tags: [镜像]
---

科大源

```
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

清华源

```
sudo sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list
```

