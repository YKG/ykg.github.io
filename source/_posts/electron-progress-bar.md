---
title: Electron进度条
date: 2019-09-15 11:55:36
tags: [Electron]
---

先是使用了`electron-progressbar`，但这个用起来质量很差，最后放弃了，转而使用简单的[官方版本][1]，只在任务栏上先是进度，且不提供中断。

```javascript
const { BrowserWindow } = require('electron')
const win = new BrowserWindow()

win.setProgressBar(0.5)
```

[1]: https://electronjs.org/docs/tutorial/progress-bar
