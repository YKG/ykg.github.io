---
title: ip2region
date: 2019-07-29 21:18:59
tags: [GeoIP]
---

今天使用了两个地址位置库，先是nodejs的`geoip-lite`，库有130+M，之后导演说他在`ip2region`,我也弄了下，库在6+M，中文，速度也很快。在我用的这台机器上测试，geoip-lite查询一个时间在1.5ms的样子，使用ip2region的btreeSearch时间在0.3ms的样子，使用memorySearch时间在0.05ms的样子，速度比大约`1:5:30`。
