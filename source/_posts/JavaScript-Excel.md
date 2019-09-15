---
title: JavaScript Excel
date: 2019-09-15 12:00:05
tags: [JavaScript, Excel]
---

折腾了半天，结果js-xlsx不能加样式，只有Pro收费版提供，放弃，转而使用exceljs。

exceljs没有range支持，设置一个range的背景色只能自己写循环，一个一个cell地配。

```JavaScript
const bgColorHeader = {
    type: 'pattern',
    pattern:'solid',
    fgColor:{argb:'FFFFF492'}
};

const bgColorBrown = {
    type: 'pattern',
    pattern:'solid',
    fgColor:{argb:'FFFECB9C'}
};

for(let ix = 0; ix <= 20; ix++) {
    worksheet.getCell(String.fromCharCode(ix + 65) + '2').fill = bgColorHeader
    worksheet.getCell(String.fromCharCode(ix + 65) + '7').fill = bgColorBrown
}
```
