---
title: 怎样使MySQL timestamp列支持毫秒
date: 2019-07-08 21:23:41
tags: [MySQL]
---

使用`TIMESTAMP(3)`、`CURRENT_TIMESTAMP(3)`这样，同理6位的微秒括号里是6

例子：

```sql
uts timestamp(3) default CURRENT_TIMESTAMP(3) not null on update CURRENT_TIMESTAMP(3),
```
