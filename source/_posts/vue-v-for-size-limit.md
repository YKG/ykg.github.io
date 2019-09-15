---
title: vue限制v-for列表的大小
date: 2019-09-15 12:32:55
tags: [Vue]
---


比如将size限定在20个

```html
<div v-for="(item, index) in items.slice(0, 20)">
    <div>{{index}}</div>
</div>
```
