---
title: 怎样加速Mac OSX上Docker bind mount非常慢的问题
date: 2019-07-05 19:12:07
tags: [Docker]
---

使用mutagen

### 使用

```bash
ALL_PROXY=socks5://127.0.0.1:1086 brew install mutagen # 安装, 也可以在github release页面下载编译好的版本

mutagen create ./src docker://www-data@fpm_app_1/var/www/html # 监视 本地./src <===> 容器name/var/www/html 并同步，使用www-data作为用户

mutagen ls # 查看create创建的同步session，会有uuid的session id

mutagen terminate ffff-ffff-ffff-ffff-ffffff # 终止某个同步
```

### 遇到的问题：

- docker://[user@]container-name 这个只能做到user可以指定，但不能指定user group
