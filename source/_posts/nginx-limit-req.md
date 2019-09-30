---
title: nginx limit req
date: 2019-09-28 08:49:56
tags:
---

```nginx.conf
geo $limit {
    default 1;
    127.0.0.1/32 0;
    172.21.0.5/32 0;
    192.168.22.11/24 0;
}

map $limit $limit_key {
    0 "";
    1 $binary_remote_addr;
}

limit_req_zone $limit_key zone=perip:10m rate=5r/s;
```


```

failregex = (?i)^<HOST> -.*(GET|POST|HEAD).*(\.php|\.asp|\.exe|\.pl|\.cgi|\.scgi|\.exp)

```