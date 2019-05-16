---
title: fail2ban
date: 2019-05-16 21:16:02
tags:
---

之前也注意到Azure的虚拟机内容使用量比较高，是一个叫`fail2ban`的程序，查询得知是安全相关，有22端口开放，有人暴力测试登陆ssh。于是改了端口号，重启了`fail2ban`。

之前的htop结果：
![](before__WX20190516-154527@2x.png)

改完端口，重启`fail2ban`之后:
![](after__WX20190516-211440@2x.png)

#### 重启fail2ban
```shell
# systemctl status fail2ban.service
systemctl restart fail2ban.service
```
