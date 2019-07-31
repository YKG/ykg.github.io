---
title: 怎样在Linux中没有编辑器的情况下修改文件
date: 2019-07-31 13:12:25
tags: [Linux]
---

原因是在docker中，可能vim没有安装，nano也没有。之前尝试apt安装，但是因为dns server的问题安装不了，开始没有去解决这个问题，然后知道有可以使用cat带`EOF`标记之类的方法进行输入的，查了下，记录在这里：

```
cat <<EOF >>test.txt
这是一段测试文字
This is Line 2.
EOF
```

然后cat test.txt

```
这是一段测试文字
This is Line 2.
```

另一种是使用sed工具替换一些内容。
