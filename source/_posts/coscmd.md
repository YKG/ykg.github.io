---
title: coscmd
date: 2019-07-18 20:03:38
tags: [COS]
---

腾讯云cos提供coscmd支持命令行形式进行cos对象操作。我的需求是希望将文件重命名，但怕影响旧数据，就同时复制并重新命名。
例如：

```bash
# 将 img 桶（chengdu）的 fromFileName.jpg 拷贝到 img-bj 桶（beijing）中，新文件名为toFileName
coscmd -b img-bj-1255000004 -r ap-beijing copy img-1255000004.cos.ap-chengdu.myqcloud.com/fromFileName.jpg toFileName
```
