---
title: IndexedDB & Dexie.js
date: 2019-09-15 12:18:17
tags: [IndexedDB]
---

IndexedDB容量没有精确值或百分比，参考[Storage limits][1]，浏览器的总存储容量是硬盘可用空间的1/2。
参考这个[回答][2]，可以使用`navigator.storage.estimate()`来看quota

mdn上给的IndexedDB包装库第一位是`localForage`，第二是`Dexie.js`，先用了localForage，但api对于便利和过滤似乎支持不够，转而使用Dexie.js。

dexie不能使用Boolean字段作为索引，比如开始有个`hide`的boolean标志位，想用来过滤使用，但发现不能在DevTools的indexeddb视图中看见用hide作为索引的结果，于是改为使用number型，用0和1来表示false和true。

在现在这台差不多顶配的MacBook Pro（Retina, 15-inch, Mid 2015）上，读写一条记录的延时在20ms左右。拉取一个小于几十条记录的列表，是80ms。



[1]: https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Browser_storage_limits_and_eviction_criteria#Storage_limits
[2]: https://stackoverflow.com/questions/26257183/detecting-available-storage-with-indexeddb

