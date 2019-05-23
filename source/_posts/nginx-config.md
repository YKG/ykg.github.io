---
title: nginx config
date: 2019-05-23 16:43:30
tags:
---

- client intended to send too large body
```nginx.conf
http {
    client_max_body_size 20M;
}
```