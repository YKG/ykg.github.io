---
title: 'JSON parse error: Cannot deserialize value of type `java.time.LocalDateTime` from String'
date: 2019-07-08 21:16:11
tags: [Java]
---

错误
```
JSON parse error: Cannot deserialize value of type `java.time.LocalDateTime` from String "2019-06-07 13:16:58": 
    Failed to deserialize java.time.LocalDateTime: (java.time.format.DateTimeParseException)
```

解决：

将字符串中间的空格改为'T': "2019-06-07T13:16:58"就可以通过
