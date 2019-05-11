---
title: 使用腾讯云的数据万象为图片添加水印
date: 2019-05-11 20:36:48
tags: 腾讯云, 数据万象
---

下面的例子是将图片进行50%缩放后添加水印持久化保存在COS中

#### 请求

```http
POST http://img-1255000004.pic.ap-chengdu.myqcloud.com/fc3v1gv47u.jpg?image_process
Authorization: q-sign-algorithm=sha1&q-ak=AKIDMauCpHF4Zr360jrbs5c2LYotiL065zbj&q-sign-time=1557566660;1557570260&q-key-time=1557566660;1557570260&q-header-list=&q-url-param-list=&q-signature=4098faff1314b571f60f2c8bfe2e719933508886
Pic-Operations: {"is_pic_info":1,"rules":[{"fileid":"wm.jpg","rule":"imageMogr2/thumbnail/!50p|watermark/2/text/546v5aGUMjAxOQ==/fontsize/100/fill/I2ZmZmZmZg==/dissolve/50/gravity/center"}]}
```

#### 响应

```xml
<UploadResult>
    <OriginalInfo>
        <Key>fc3v1gv47u.jpg</Key>
        <Location>img-1255000004.cos.ap-chengdu.myqcloud.com/fc3v1gv47u.jpg</Location>
        <ImageInfo>
            <Format>JPEG</Format>
            <Width>1620</Width>
            <Height>1080</Height>
            <Quality>93</Quality>
            <Ave>0x4f5033</Ave>
            <Orientation>0</Orientation>
        </ImageInfo>
    </OriginalInfo>
    <ProcessResults>
        <Object>
            <Key>wm.jpg</Key>
            <Location>img-1255000004.cos.ap-chengdu.myqcloud.com/wm.jpg</Location>
            <Format>JPEG</Format>
            <Width>810</Width>
            <Height>540</Height>
            <Size>109507</Size>
            <Quality>93</Quality>
        </Object>
    </ProcessResults>
</UploadResult>
```


#### 在小程序中应用例子


```javascript
waterMark(key) {
    const that = this;

    const options = {
      Method: 'post',
      Pathname: '/' + key,
      Expires: 60
    };

    const authorization = COS.getAuthorization({
      SecretId: 'AKIDMauCpHF4Zr360jrbs5c2LYotiL065zbj',
      SecretKey: 'FMT6dzBIFKKvjRxc4SFraUPI3KpNOAlB',
      Method: options.Method,
      Pathname: options.Pathname,
      Query: options.Query,
      Headers: options.Headers,
      Expires: 60,
    });

    const thumb = key.replace('.', '.wm.')
    wx.request({
      method: 'post',
      url: 'https://img-1255000004.pic.ap-chengdu.myqcloud.com/' + key + '?image_process',
      header: {
        'Authorization': authorization,
        'Pic-Operations': JSON.stringify({ "is_pic_info": 1, "rules": [{ "fileid": thumb, "rule": "imageMogr2/thumbnail/!50p|watermark/2/text/546v5aGUMjAxOQ==/fontsize/100/fill/I2ZmZmZmZg==/dissolve/50/gravity/center" }] }) 
      },
      success(res) {
        // console.log(res.data)
      },
      fail(err) {
        wx.showModal({
          title: 'err',
          content: JSON.stringify(err),
        })
        console.log(err)
      }
    })
  },
```