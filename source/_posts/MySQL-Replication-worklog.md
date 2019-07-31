---
title: MySQL复制操作日志
date: 2019-07-31 12:55:52
tags: [MySQL]
---

今天把bj服务器上的mysql做了主备复制到nas上，这里记录下这个过程。

因为线上服务器已经有了之前的数据，现在才开始开启bin-log，发现slave上start slave之后，只会有之后的同步，之前的数据并没有。需要将原先的数据dump出来再在slave上导入。

在master上：

```bash
mysqldump -uroot -p --all-databases --master-data --single-transaction > all_databases.sql
```

在slave上：

```bash
mysql -uroot -p < all_databases.sql
```

但结果出现错误

```
MYSQL DUMP ERROR 1726 Storage engine 'InnoDB' does not support system tables. [mysql.columns_priv]
```

后来发现是两个db的版本不一致。master是8.0.16，slave是5.7.17。之后就折腾升级slave机器的版本，折腾了半天也没弄好，倒是好不容易撞上了8.0，但是装的则是8.0.17，而且启动还是有问题，说是找不到一个`mysql-helper`的什么东西，最后不折腾了，还是按照docker来，弄一模一样的。master上的是在docker-compose.yml里定义的，所以直接拷来，运行了一个新的初始环境，导入也成功了。

但后面change master的时候，不知道posistion应该填啥，看过去的master执行记录，较早的show master status的执行结果是在155的位置，当前的结果是500+，slave上就按照155指定的。然后检查数据都在，尝试一个无用的表插入数据，也同步来了，就算ok了。

回顾：
- 应该一开始就使用docker，而不是自己尝试去升级，浪费了几个小时，升级还没成。
- linux/ubuntu的包管理还是不了解，全是按文档瞎试
- 想升级是为了也积累一点儿升级知识，但这样会耽误当前主要任务的，应该确保关键路径快速通过，不要为细枝末节阻塞关键路径
