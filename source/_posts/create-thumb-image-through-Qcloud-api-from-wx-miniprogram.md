---
title: 如何在小程序中调用腾讯云数据万象API创建图片缩略图
date: 2019-05-10 19:24:33
tags: 微信小程序
---

```javascript
const COS = require('../../lib/cos-wx-sdk-v5');

const Key = '<filename>.jpg';

const config = {
    SecretId: 'AKID<xxx>',
    SecretKey: '<xxxx>',
    urlPrefix: 'https://<BucketName>-<APPID>.pic.<Region>.myqcloud.com/' // e.g. img-1255000000.pic.ap-chengdu.myqcloud.com
};

const authorization = COS.getAuthorization({
    SecretId: config.SecretId,
    SecretKey: config.SecretKey,
    Method: 'post',
    Pathname: '/' + Key,
    Expires: 60,
});

wx.request({
    method: 'post',
    url: config.urlPrefix + Key + '?image_process',
    header: {
        'Authorization': authorization,
        'Pic-Operations': JSON.stringify({ "is_pic_info": 1, "rules": [{ "fileid": Key.replace('.', '.240x180.'), "rule": "imageView2/1/w/240/h/180/q/85" }] })
    },
    complete(res) {
        console.log(res)
    }
})
```