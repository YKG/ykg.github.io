---
title: port forwaring by nginx
date: 2019-06-27 23:00:43
tags: [Nginx, Port Forwarding]
---

想用iptables来做，没成功，原因未知。用nginx的反向代理做的。之前用redir的docker，结果发现nas的资源管理器显示大量redir的僵尸进程。

mkdir nginx

目录下只需要两个文件docker-compose.yml和nginx.conf

cd nginx

vi docker-compose.yml 

```Dockerfile
version: '3'
services:
  nginx_tcp_relay:
    image: nginx:alpine
    ports:
     - "12345:12345"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
```

vi nginx.conf

```conf
user  nginx;
worker_processes  1;

events {
    worker_connections  1024;
}

stream {
    upstream shadowsocks{
        server vps.domain.net:12345;  # ip:port
    }

    server {
        listen 12345;
        proxy_pass shadowsocks;
    }
}
```
