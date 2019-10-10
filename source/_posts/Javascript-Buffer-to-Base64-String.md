---
title: JavaScript Base64
date: 2019-10-10 20:10:02
tags: [JavaScript, Base64]
---

```javascript
// 浏览器端
let arr = [128, 28, 29, 102, 392]
btoa(String.fromCharCode.apply(null, arr))

// Node.js
Buffer.from("Hello World").toString('base64')
```
