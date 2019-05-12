---
title: 获取图片EXIF信息
date: 2019-05-12 20:59:00
tags: [数据万象, API]
---

腾讯的数据万象API接口：[链接][1]  

例子给的url是这样的:
```
http://examples-1251000004.picsh.myqcloud.com/sample.jpeg?exif
```

我用自己的存储里的图片，却一直不成功，比如
```
http://myimages-1000000004.cos.ap-chengdu.myqcloud.com/sample.jpeg?exif
```
和
```
http://myimages-1000000004.pic.ap-chengdu.myqcloud.com/sample.jpeg?exif
```
和
```
http://myimages-1000000004.picsh.ap-chengdu.myqcloud.com/sample.jpeg?exif
```
和
```
http://myimages-1000000004.picsh.myqcloud.com/sample.jpeg?exif
```

后来进去数据万象的控制台，看图片的地址是这样的：
```
http://myimages-1000000004.piccd.myqcloud.com/sample.jpeg?exif
```

这下才明白腾讯官网的哪个例子没说清楚，地区简写是跟在`pic`后面的，一直没明白`picsh`是什么意思，看到`piccd`就明白了。`sh`应该是上海，`cd`对应成都。后来给腾讯云的文档中心对应文档页提了反馈，建议增加两个例子，这样就清楚了，免得浪费更多的人的时间。


[1]: https://cloud.tencent.com/document/product/460/6926


