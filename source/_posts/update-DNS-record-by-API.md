---
title: 通过API更新阿里云域名记录解析
date: 2019-05-06 10:25:00
tags:
---

使用Aliyun的[OpenAPI Exploer][1]可以直接拿来测试  

accessKeyId 和 accessKeySecret 需要去控制台`RAM访问控制`中创建一个新用户，添加`AliyunDNSFullAccess`权限  

`OpenAPI Exploer`做得很不错


## DDNS

有了上面的东西，就可以做一个DDNS了。

关键代码：
```javascript
var params = {
    "RecordId": "100000100877960192",
    "RR": "nas",
    "Type": "A",
    "Value": "1.1.1.1"
}

function updateDNSRecord(ip) {
    params.Value = ip;
    client.request('UpdateDomainRecord', params, requestOption).then((result) => {
        console.log(result);
    }, (ex) => {
        console.log(ex);
    })
}

if (lastIP.ip !== ip) {
    updateDNSRecord(ip);
}
```



[1]: https://api.aliyun.com/?spm=a2c4g.11186623.2.25.3c4d2846uWi4cX#/?product=Alidns&api=UpdateDomainRecord&params={%22RegionId%22:%22default%22,%22RecordId%22:%220000%22,%22RR%22:%22nas%22,%22Type%22:%22A%22,%22Value%22:%221.1.1.1%22}&tab=DEMO&lang=NODEJS
