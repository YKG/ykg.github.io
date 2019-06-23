---
title: 怎样获取mysql docker的root密码
date: 2019-06-23 22:13:05
tags: [MySQL, docker, password]
---

[参考链接][1]

```
sudo docker exec mysql-docker-container env
```

或者如果是docker-compose启动话，也可以看

```yaml
services:
    db:
        environment:
            MYSQL_ROOT_PASSWORD: 'password'
```

```
sudo docker-compose run db env
```

当然也可以直接检查`docker-compose.yml`



[1]: https://github.com/docker-library/mariadb/issues/99#issuecomment-281627759