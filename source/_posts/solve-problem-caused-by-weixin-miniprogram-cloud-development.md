---
title: 解决小程序云开发不支持数据库事务引发的问题
date: 2019-05-27 23:02:19
tags:
---

小程序有上传图片功能，但考虑到图片可能由非手机设备拍摄的，因此增加了Web上传接口。Web上传后，小程序中有一个数据同步按钮，用以触发云函数去拉取Web上的数据。结果发现有大量重复数据被插入到小程序云数据库中，临时方案进行去重处理，代码如下：

```javascript
let all = []
for (let i  = 0; i < 10; i++) {
    let res = await db.collection('product').skip(i * 100).limit(100).get()
    console.log(res)
    if (res.data.length === 0) {
        break
    }
    all = all.concat(res.data)
}

console.log(all)
const obj = {}
all.forEach(e => {
    obj[e.fileId] =  true
})

all.forEach(function(e){
    if (obj[e.fileId]) {
        delete obj[e.fileId]
    } else {
        if (e.visible === undefined) {
        console.log('delete ', e.fileId)
        db.collection('product').doc(e._id).remove()
        }
    }
})
```
