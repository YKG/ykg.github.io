---
title: nginx防御
date: 2019-09-27 17:08:08
tags: [nginx, 安全]
---

### 步骤

- 安装fail2ban
- 给nginx加流控
- 配置fail2ban
- 重启nginx、fail2ban


### 流控

    ```conf
    #---------------------------------------------
    # /etc/nginx/nginx.conf

    http {
        limit_req_zone $binary_remote_addr zone=perip:10m rate=5r/s;
        limit_req_zone $server_name zone=perserver:10m rate=50r/s;
        limit_req zone=perip burst=5 nodelay;
        limit_req zone=perserver burst=50 nodelay;
        limit_req_status 444;

        ; server {
        ;     limit_req zone=perip burst=5 nodelay;
        ;     limit_req zone=perserver burst=50 nodelay;
        ;     limit_req_status 444;
        ;     #limit_req_log_level warn;
        ; }
    }
    
    #---------------------------------------------
    # /etc/fail2ban/jail.d/nginx-limit-req.local

    [nginx-limit-req]
    enabled = true
    ```

### 禁止脚本后缀

    ```conf
    #---------------------------------------------
    #  /etc/fail2ban/filter.d/nginx-noscript.local

    [Definition]

    failregex = ^<HOST> -.*GET.*(\.php|\.asp|\.exe|\.pl|\.cgi|\.scgi|\.exp)

    ignoreregex =

    #--------------------------------------------
    # /etc/fail2ban/jail.d/nginx-noscript.local

    [nginx-noscript]

    enabled  = true
    port     = http,https
    filter   = nginx-noscript
    logpath  = /var/log/nginx/access.log
    bantime = 48h
    findtime = 60
    maxretry = 3
    ```

### 重启nginx、fail2ban

```bash
sudo nginx -s reload

sudo fail2ban-client reload # sudo service fail2ban restart

# sudo fail2ban-client status # 检查jail列表
```

### 测试验证

测试前调整

```conf
# /etc/nginx/nginx.conf

limit_req_zone $binary_remote_addr zone=perip:10m rate=5r/m; # 为了实验方便，改成 5r/m
```

测试机

```bash
# 测试机器发送请求
x=1; while [ $x -le 10 ]; do curl https://host/; done
```


服务器检查

```bash
## 服务器
# 测试前
$ sudo fail2ban-client status nginx-limit-req
Status for the jail: nginx-limit-req
|- Filter
|  |- Currently failed:	0
|  |- Total failed:	0
|  `- File list:	/var/log/nginx/error.log
`- Actions
   |- Currently banned:	0
   |- Total banned:	0
   `- Banned IP list:


# 测试后
$ sudo fail2ban-client status nginx-limit-req
Status for the jail: nginx-limit-req
|- Filter
|  |- Currently failed:	1
|  |- Total failed:	8
|  `- File list:	/var/log/nginx/error.log
`- Actions
   |- Currently banned:	1
   |- Total banned:	1
   `- Banned IP list:	1.2.3.4

$ tail /var/log/nginx/error.log
2019/09/27 18:04:55 [notice] 8836#8836: signal process started
2019/09/27 18:22:10 [notice] 11834#11834: signal process started
2019/09/27 18:22:18 [error] 11835#11835: *1126 limiting requests, excess: 5.911 by zone "perip", client: 218.4.167.126, server: target-host, request: "GET / HTTP/1.1", host: "target-host"
2019/09/27 18:22:18 [error] 11835#11835: *1127 limiting requests, excess: 5.891 by zone "perip", client: 218.4.167.126, server: target-host, request: "GET / HTTP/1.1", host: "target-host"
2019/09/27 18:22:18 [error] 11835#11835: *1128 limiting requests, excess: 5.879 by zone "perip", client: 218.4.167.126, server: target-host, request: "GET / HTTP/1.1", host: "target-host"
2019/09/27 18:22:18 [error] 11835#11835: *1129 limiting requests, excess: 5.864 by zone "perip", client: 218.4.167.126, server: target-host, request: "GET / HTTP/1.1", host: "target-host"
2019/09/27 18:22:19 [error] 11835#11835: *1130 limiting requests, excess: 5.849 by zone "perip", client: 218.4.167.126, server: target-host, request: "GET / HTTP/1.1", host: "target-host"

# 取消ban
# sudo fail2ban-client set nginx-http-auth unbanip 111.111.111.111 # 没有测试，默认bantime是10m，等它自动解封
$ date
Fri Sep 27 18:34:36 CST 2019
$ sudo fail2ban-client status nginx-limit-req
Status for the jail: nginx-limit-req
|- Filter
|  |- Currently failed:	1
|  |- Total failed:	8
|  `- File list:	/var/log/nginx/error.log
`- Actions
   |- Currently banned:	0
   |- Total banned:	1
   `- Banned IP list:
```

### 其他

专门看了第一篇参考文献，burst专门看了下。


### 参考文献

- [Rate Limiting with NGINX and NGINX Plus][1]
- [How To Protect an Nginx Server with Fail2Ban on Ubuntu 14.04][2]
- [fail2ban-client][3]
- [Module ngx_http_geo_module][4]
- [Module ngx_http_limit_req_module][5]

[1]: https://www.nginx.com/blog/rate-limiting-nginx/
[2]: https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04
[3]: https://www.fail2ban.org/wiki/index.php/Commands
[4]: https://nginx.org/en/docs/http/ngx_http_geo_module.html
[5]: http://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req_status

