---
title: NginX反向代理配置
date: 2019-05-21 17:52:45
tags:
---

[参考][1]
[官网文档][2]


```nginx.conf
location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass http://127.0.0.1:3000;
    proxy_redirect off;
}
```


[1]: https://www.digitalocean.com/community/questions/nginx-reverse-proxy-with-express-and-node-js-app-not-redirecting-to-https
[2]: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
