---
title: nginx防御2
date: 2019-09-29 10:05:01
tags: [nginx, 安全]
---


### 禁止发送server版本号

```
http {
    server_tokens off;
}
```


### location 后缀白名单

```conf
server {
        location / {    # 这个地方是不是还应该进一步加强，恶意输入的path前缀可能不是`/`，比如`0x01`
            return 444;
        }
        location = / {
            root /data/www;
        }
        location ~ \.(html|css|js|jpe?g|png|gif|ico|txt|woff|ttf|eot|svg)$ {
            root /data/www;
        }
}
```


### 强制https

```conf
server {
    listen 80;
    listen [::]:80;

    server_name hostname;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name hostname;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"; # 另有always参数
}
```


### 请求方法白名单

```conf
server {
    if ( $request_method !~ ^(GET|POST|HEAD)$ ) {
        return 405;
    }
}
```

> 想继续ban掉不在白名单的请求ip


### 白名单带有请求参数的请求

```conf
server {
    if ($args !~ '^(|v=[0-9]+)$') {
        return 403;
    }
}
```

### cgi

似乎不用配置,要显示指明`fastcgi_pass`才会用到，像是和`proxy_pass`类似

### https优化

```
http {
    server {
        ssl_session_cache   shared:SSL:10m;
        ssl_session_timeout 10m;
        # ssl_handshake_timeout 15s; # 这个配了，报 nginx: [emerg] "ssl_handshake_timeout" directive is not allowed here
    }
}
```

参考[官方文档][1]


### naxsi

- [ ] 需要配合nginx源码编译，延迟加入

### 禁止bot

```conf
# /robots.txt
User-agent: *
Disallow: /
```

### favicon.ico 404

```
location = /favicon.ico {
    return 404;
}
```

这样就不会在error.log里看到fopen错误了


[1]: https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/#https-server-optimization
