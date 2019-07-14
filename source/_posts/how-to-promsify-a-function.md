---
title: how to promsify a function
date: 2019-07-14 08:20:34
tags: [JavaScript]
---

原函数：

```javascript
cos.postObject({
    Bucket: config.Bucket,
    Region: config.Region,
    Key: Key,
    FilePath: filePath,
}, function (err, data) {
    if (err) {
        wx.showToast({ title: '上传图片错误', icon: 'none' })
        console.log(err);
        return;
    }
    wx.showToast({ title: '上传图片成功', icon: 'success' })
});
```

改造后：

```javascript
async function saveToCOS(filePath) {
    const result = {ok: true, data: ""}
    var Key = util.getRandFileName(filePath);

    function cosPostObject(resolve, reject) {
      cos.postObject({
        Bucket: config.Bucket,
        Region: config.Region,
        Key: Key,
        FilePath: filePath,
      }, function (err, data) {
        if (err) {
          reject(err)
        } else {
          resolve(data)
        }
      })
    }

    function errorHandler(err) {
      result.ok = false
      result.data = err
    }

    const res = await new Promise(cosPostObject).catch(errorHandler)

    if (res) {
      result.ok = true
      result.data = res
    }
    return result
}

// caller
const res = await saveToCOS(...)
if (res.ok) {
    //...
} else {
    //...
}
```

### Update

原来可以这么写：

```javascript
async function foo() {
    return await rp('http://googe.com').then(() => {ok: true}).catch(() => {ok: false})
}
```

之前以为只能这样：

```javascript
async function foo() {
    const result = {}

    function errorHandler(err) {
      result.ok = false
      result.data = err
    }

    const res = await rq('http://googe.com').catch(errorHandler)

    if (res) {
      result.ok = true
      result.data = res
    }
    return result
}
```

