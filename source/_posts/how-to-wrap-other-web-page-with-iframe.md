---
title: 怎样使用iframe套用其他网页
date: 2019-08-08 17:03:29
tags: [HTML]
---

```html
<iframe style="position:fixed; top:0; left:0; bottom:0; right:0; width:100%; height:100%; border:none; margin:0; padding:0; overflow:hidden; z-index:999999;" src="" frameborder="0"></iframe>
```

为什么使用这个东西

- 因为腾讯云的服务器带宽有限，将具体服务搬至家用ADSL上网的NAS机器上
- 为避免不必要的麻烦，NAS使用非80等常用端口，使用高位端口
- 带端口的看起来不友好，为了向用户提供不带端口的入口，需要进行再包装
- 使用iframe方式是最省事的方式
- 直接食用iframe的时候，iframe窗口很小，使用上面的样式看起来和不带iframe的一样
