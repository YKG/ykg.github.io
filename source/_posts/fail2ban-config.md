---
title: fail2ban配置
date: 2019-09-30 09:03:41
tags: [fail2ban, 安全]
---

```conf
# /etc/fail2ban/filter.d/nginx-whitelist-server-name.local
[nginx-whitelist-server-name]

enabled  = true
port     = http,https
filter   = nginx-whitelist-server-name
logpath  = /var/log/nginx/access.log
bantime = 48h
findtime = 60
maxretry = 1
```


```conf
# /etc/fail2ban/filter.d/nginx-whitelist-server-name.local
[Definition]

failregex = sn="(_|_ssl)" rt=

ignoreregex =
```

执行重新加载结果

```bash
$ sudo fail2ban-client reload
 NOK: ('No failure-id group in \'sn="(_|_ssl)" rt=\'',)
No failure-id group in 'sn="(_|_ssl)" rt='
```

不知道问题，后来明白了，fail2ban不知道如何确定去ban谁，failregex改为下面即成功。

```conf
# sn="(_|_ssl)" rt=             # 原来错误的情况
^<HOST> -.*sn="(_|_ssl)" rt=
```


发现应该被封的ip，还是可以访问，检查iptables，发现没有rules，后来检查fail2ban.log，看到了这个：

```
stderr: "iptables v1.6.1: Invalid target name `f2b-nginx-whitelist-server-name' (28 chars max)"
```

```
$ sudo fail2ban-client stop nginx-whitelist-server-name
Jail stopped
$ sudo fail2ban-client status
Status
|- Number of jail:	4
`- Jail list:	nginx-limit-req, nginx-noscan, nginx-noscript, sshd
```
