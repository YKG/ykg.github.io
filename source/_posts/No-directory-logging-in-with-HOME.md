---
title: MySQL 5.7 'No directory, logging in with HOME=/'
date: 2019-07-30 21:03:52
tags: [MySQL]
---

遇到题目上的错误，看了[这篇问答][1]，解决方法：

```bash
sudo service mysql stop
sudo usermod -d /var/lib/mysql/ mysql
```

[1]: https://askubuntu.com/questions/737903/mysql-5-7-no-directory-logging-in-with-home
