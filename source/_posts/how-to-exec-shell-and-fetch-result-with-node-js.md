---
title: 怎样用Node.js执行一个shell命令并获取命令结果
date: 2019-07-26 22:20:00
tags: [NodeJS]
---

例子：

```javascript
// 从 coscmd info <fileId> 命令的结果中抽取 Last-Modified 时间
const info = spawn('coscmd', ['info', cosId]);
info.stdout.on('data', (data) => {
    data = data.toString();
    let i = data.indexOf("Last")
    let j = data.indexOf("GMT")
    let s = data.substring(i + 22, j) + "GMT"
    console.log(new Date(s))
})
```

Node.js[官网链接][1]

[1]: https://nodejs.org/api/child_process.html#child_process_child_process
