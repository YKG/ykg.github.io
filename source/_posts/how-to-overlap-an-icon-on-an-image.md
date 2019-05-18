---
title: 怎样在一张图片上叠加图标
date: 2019-05-18 20:51:35
tags: [CSS]
---

通过把父容器设置为`position: relative`，再对图标元素设置`position: absolute; top: 10px; right: 10px;`即可。[参考][1]

例：

HTML: 
```html
<view class="outer">
    <view class='question'>?</view>
    <image src="{{url}}"></image>
</view>
```

CSS:
```css
.outer {
  position: relative;
}

.question {
  position:absolute;
  top:10px;
  right:10px;
  border-radius:50%;
  border:1px solid #DDD;
  width:16px;
  height:16px;
  font-size:11px;
  color:#DDD;
  opacity: 0.5;
}
```

效果图，右上角叠加一个问号：
![](demo-WX20190518-205746@2x.png)


[1]: https://stackoverflow.com/questions/23238108/how-to-add-button-over-image-using-css