---
title: 添加Disqus评论
date: 2019-05-08 22:10:48
tags:
---

参考这个[链接][1]注册了Disqus账号，然后在根目录下的`_config.yml`上填上了`shortname`，但部署后刷新很多次不生效。  

以为要在页面上配置什么，但之后找到主题的[说明文档][2]，似乎并没有说需要那么做。
最后找了主题目录翻到了`themes/maupassant/_config.yml`里面找到了和说明文档一样的配置文件，填写了disqus配置，部署后生效。  

但生效的情况是

```
Disqus 无法加载。如果您是管理员，请参阅故障排除指南
```

找到[这篇文章][3]给根目录的url配上`http://`前缀，原先的值只是域名，应用后，总算正常工作了。


### 问题

为了把修改记录到git，但引发了`submodule`问题，等研究明白再写一篇。

- 错误提示1 ([参考][3])

```
 fatal: Pathspec themes/maupassant is in submodule
```

- 错误提示2: `git status`

```
  (commit or discard the untracked or modified content in submodules)

	modified:   themes/maupassant (modified content)
```


[1]: http://www.cylong.com/blog/2017/03/26/hexo-next-disqus/
[2]: https://www.haomwei.com/technology/maupassant-hexo.html
[3]: https://stackoverflow.com/questions/24472596/git-fatal-pathspec-is-in-submodule
