---
title: 'openapi.templateMessage.send:fail invalid form id'
date: 2019-05-26 23:34:08
tags: [小程序错误]
---

使用小程序的模版通知，结果得到题目的这种错误。发现如果发给支付方则可以成功，而想发送通知给售出方则会有下面的这种错误。

```json
{"errCode":41028,"errMsg":"openapi.templateMessage.send:fail invalid form id hint: [xbWplA05654114]"}
```

有的说线上可以正常发送，因为没上线，等上线了再确认下。
