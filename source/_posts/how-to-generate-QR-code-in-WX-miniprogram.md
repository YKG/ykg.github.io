---
title: 怎样生成小程序二维码
date: 2019-06-21 21:56:50
tags: [小程序]
---

[官方文档][1]

- 要在服务端使用`openapi.wxacode.getUnlimited`来生成图片
- 需要permissions配置才能使用
- 图片放到云存储里，cloudPath用base62编码了一下，看文档对于`=`字符腾讯那边似乎会处理，所以不用base64
- base62.encode参数是接受Buffer
- `scene`用来存放参数，`page`只能存储路径

服务端：

```javascript
// 云函数入口文件
const cloud = require('wx-server-sdk')
const BASE62 = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
const base62 = require('base-x')(BASE62)

cloud.init()

// 云函数入口函数
exports.main = async (event, context) => {
  const wxContext = cloud.getWXContext()

  const { args } = event

  try {
    result = await cloud.openapi.wxacode.getUnlimited({
      scene: args,
      page: 'pages/album/byuser'
    })
    console.log(result)

    const cloudPath = base62.encode(Buffer.from(args, 'utf8'))
    console.log(args, cloudPath)

    let fileId = await cloud.uploadFile({
      cloudPath: cloudPath,
      fileContent: result.buffer,
    })

    result.fileId = fileId
    return result
  } catch (err) {
    console.log(err)
    return err
  }
}
```

另外需要在云函数目录加入`config.json`:

```json
{
  "permissions": {
    "openapi": [
      "wxacode.getUnlimited"
    ]
  }
}
```


客户端：

```javascript
genPoster: async function () {
    const self = this;

    const resQR = await  wx.cloud.callFunction({
      name: 'share',
      data: {
        args: 'pages/album/byuser'
      }
    })

    const qrUrl = resQR.result.fileId.fileID
    console.log(qrUrl)

    wx.cloud.downloadFile({
      fileID: qrUrl,
      fail(err) {
        console.log(err)
      },
      success(qrres) {
        if (qrres.statusCode === 200) {
            console.log(qrres.tempFilePath) // 二维码路径
        }
      }
    }
}
```


[1]: https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/qr-code/wxacode.getUnlimited.html
