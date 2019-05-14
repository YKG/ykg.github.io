---
title: 怎样处理await语句后面Promise的reject
date: 2019-05-14 21:28:03
tags: JavaScript
---

使用`try/catch`结构来处理  

MDN上的一个[例子][1]
```javascript
async function f3() {
  try {
    var z = await Promise.reject(30);
  } catch(e) {
    console.log(e); // 30
  }
}
f3();
```

StackOverflow上的[回答][2]


[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await
[2]: https://stackoverflow.com/questions/42453683/how-to-reject-in-async-await-syntax
