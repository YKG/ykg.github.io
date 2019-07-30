---
title: 怎样修改MySQL配置
date: 2019-07-30 21:14:23
tags: [MySQL]
---

新建`/etc/mysql/myopt.cnf`文件，并将改动部分写入该文件，之后在`/etc/mysql/my.cnf`中追加一句把这个文件include进来：

```
!include /etc/mysql/myopt.cnf
```

例如仅仅修改一下绑定地址和端口

```conf
# /etc/mysql/myopt.cnf
[mysqld]
bind-address 	= 0.0.0.0
port            = 50006
```
