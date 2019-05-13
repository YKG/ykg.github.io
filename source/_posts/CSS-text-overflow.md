---
title: CSS text-overflow
date: 2019-05-13 08:55:05
tags: [CSS, overflow]
---

为了保证布局不被毁掉，要用到overflow，但和我期望的不一样，后来找到了[`text-overflow`][1]属性，弄好了。

```css
text-overflow: ellipsis;
overflow: hidden;
white-space: nowrap;
```

[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/text-overflow