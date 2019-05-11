---
title: 解决小程序只在调试模式下正常工作的问题
date: 2019-05-10 19:38:15
tags: 微信小程序
---

问题：小程序开启调试模式，正常工作，关闭调试模式，正常模式下，有些请求并没有发出去

解决：

- 关闭小程序开发工具的`不校验合法域名、web-view（业务域名）、TLS 版本以及 HTTPS 证书`，所有域名启用`https`
- 还有问题是域名要加入到网页小程序控制台`开发设置`--`服务器域名`各相关部分，腾讯自己的`*.myqcloud.com`都可以直接加入，问题是要等一段时间才能生效，可能10分钟左右


[参考1][1]
[参考2][2] 2019-05-11 12:06:15 update


[1]: https://developers.weixin.qq.com/community/develop/doc/55b36a507860cac08c2d9791f0df2aa6
[2]: https://developers.weixin.qq.com/community/develop/doc/e63ec2fb6639c4894471ecc9f4344933
