---
title: CNAME
date: 2019-07-02 08:53:17
tags:
---

CNAME是为了让一个域名跟踪另一个域名。

比如：

```
1.2.3.4 a.com
```

这个域名的ip记录可能会变动，想用另一个域名`b.org`也解析成`a.com`的IP，类似

```
a.com b.org
```

这种形式可以使用cname完成

b.org是为cname记录，值为a.com
