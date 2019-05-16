---
title: 怎样给Hexo中的文章添加图片
date: 2019-05-16 21:20:47
tags:
---

[参考1][1]  [参考2(官网)][2]

步骤：
1, 在`_config.yml`中启用
```yaml
post_asset_folder: true
```
2, `hexo new 'my new article'`之后，会自动新建一个`my-new-article`目录，这样只要把图片放到该目录，在文章中可以直接使用下面的形式引用图片
```
![](my-img.jpg)
```
  > 因为生成的文章目录是`my-new-article/`，所以如果图片放在那个目录了，默认的相对路径刚好是当前目录，所以说得通


[1]: https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/
[2]: https://hexo.io/docs/asset-folders.html
