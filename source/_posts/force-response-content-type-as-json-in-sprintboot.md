---
title: sprintboot设置响应头Content-Type为json
date: 2019-09-15 12:51:06
tags: [Java, SpingBoot]
---

```java
@RequestMapping(value = "/postMethod", method = RequestMethod.POST, produces = MediaType.APPLICATION_JSON_VALUE)
```
