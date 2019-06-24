---
title: 使用puppeteer进行网页截图
date: 2019-06-24 22:00:43
tags: [puppeteer]
---

[官方仓库/文档][1]

安装依赖 `npm i puppeteer`


```Javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.setViewport({
        width: 750,
        height: 5000,
        deviceScaleFactor: 1,
    });
    await page.goto('http://kaige.org');
    await page.screenshot({path: 'example.png'});

    await browser.close();
})();
```

打算将小程序海报生成由`puppeteer`截图来做

[1]: https://github.com/GoogleChrome/puppeteer
