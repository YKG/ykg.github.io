---
title: SQL权限
date: 2019-06-25 21:44:28
tags: [SQL, 权限]
---

```sql
-- As root
create database saitu;
use saitu;
grant all privileges on saitu to ykg@'%';
grant all privileges on saitu.* to ykg@'%';
```
