---
title: Nginx上配置WebSocket
date: 2019-08-29 09:23:35
tags: [Nginx]
---

下面是`wss://ssl.highlight.top/score/`的配置

```conf
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    upstream wsbackend{
        server localhost:58880;  # ws://ip:port
    }

    server {
        listen              443 ssl;
        server_name         ssl.highlight.top;
        keepalive_timeout   70;

        ssl_certificate     ssl.highlight.top.pem; # 下载自阿里云的免费ssl
        ssl_certificate_key ssl.highlight.top.key; # 这两个文件放在和nginx.conf同一目录
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;

        location /score/ {
            proxy_pass http://wsbackend;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }
    }
}
```

参考[官方文档][1],如果不需要wss，而是ws的话，从`listen`行到`location`之间的部分都可以删去。

另外的问题就是还是没明白`server`和`http`的关系，另外看到`mail{ server{} }`之类的，还有`tcp{ server{} }`，似乎`server`总是包在某一个东西下面，记得是不能独立出来的，也没找到哪里有规范进行说明。


[1]: http://nginx.org/en/docs/http/websocket.html

