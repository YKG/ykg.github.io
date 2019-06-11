---
title: TiDB学习笔记8
date: 2019-06-11 07:52:00
tags: [TiDB, TiKV]
---

运行这个命令得到
```shell
sudo docker-compose -f txnkv-docker-composer.yml pull
```

```
ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?
```


Start docker:

```shell
sudo systemctl start docker
```

晚上看了[Rust Book中文翻译][1]的前7章


[1]: https://kaisery.github.io/trpl-zh-cn/ch08-00-common-collections.html
