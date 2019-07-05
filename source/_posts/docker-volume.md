---
title: docker volume
date: 2019-07-05 19:09:03
tags: [Docker]
---

在`docker-compose.yml`中，命名一个volume

```yaml
services:
  app:
    image: nextcloud:fpm-alpine
    volumes:
      - nextcloud:/var/www/html

volumes:
  nextcloud:
```

两个service共享同一个目录

```yaml
services:
  app:
    image: nextcloud:fpm-alpine
    volumes:
      - nextcloud:/var/www/html

  web:
    build: ./web
    volumes:
       - nextcloud:/var/www/html:ro

volumes:
  nextcloud:
```

使用bind mount绑定本地（开发）目录

```yaml
services:
  app:
    image: nextcloud:fpm-alpine
    volumes:
      - ./src:/var/www/html  # 本地路径:contianer中路径

  web:
    build: ./web
    volumes:
       - ./src:/var/www/html:ro
```

避免两个volume重复指定，可以使用yaml文件的规范，[参考][1] [YAML参考][2]

```yaml
services:
  app:
    image: nextcloud:fpm-alpine
    volumes: &local-dir     # 创建ancher，下面可以使用*前缀引用
      - ./src:/var/www/html

  web:
    build: ./web
    volumes: *local-dir
```

在mac下这个bind mount目前还不行，太慢了，github上这个slow的issue一直在open。当前解决方案是避免使用bind mount，只使用named volume，volume速度和那后，可以使用mutagen来创建一个session同步两个目录


[1]: https://stackoverflow.com/a/42092050/3381650
[2]: https://blog.daemonl.com/2016/02/yaml.html

