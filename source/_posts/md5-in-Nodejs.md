---
title: Node.js获取文件MD5例子
date: 2019-08-08 17:17:03
tags: [Node.js]
---

```javascript
function md5(filename, cb) {
  const crypto = require('crypto');
  const fs = require('fs');

  const hash = crypto.createHash('md5');

  const input = fs.createReadStream(filename);
  input.on('readable', () => {
    // Only one element is going to be produced by the
    // hash stream.
    const data = input.read();
    if (data)
      hash.update(data);
    else {
      cb(hash.digest('hex'));
    }
  });
}

const filename = "/tmp/test.xt";

md5(filename, result => {
    console.log(filename + " md5: ", result)
})
```
