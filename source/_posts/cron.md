---
title: cron
date: 2019-06-25 20:32:02
tags: [基础命令]
---

用命令`crontab -e`对当前用户账户成都cron进行编辑。

增加一个每分钟执行一次的任务

```
*/1 * * * * /bin/bash /root/ddns.sh > /dev/null 2 > /dev/null
```

`crontab -l`可以列出所有条目

使用这个的原因是，之前使用的DDNS上报是通过

```shell
watch -n 30 curl http://x.x.x.x:21110/set &
```

来实现的，但存在的问题是，如果机器重启就完了。
