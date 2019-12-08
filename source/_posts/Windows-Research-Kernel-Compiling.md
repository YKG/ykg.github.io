---
title: Windows Research Kernel Compiling
date: 2019-12-08 20:33:58
tags: [WRK, Windows]
---

从GitHub下载了WRK的资源，先是用VirtualBox中的Win7x86环境编译，命令行方式通不过，报

```
\i386\debug2.asm(1) : fatal error A1000: cannot open file : obji386\debug2.obj
```

也没找到原因是啥，用vs2010编译也不行，按照说明文档中提到到vs2008编译也不行。后来换到windows server 2003 standard edition，用VMFusion装的，命令行编译没问题，编译一遍只要不到1分钟。

按照潘爱民老师的《Windows内核原理与实现》里面的boot.ini，改了win2003的boot.ini，使用wrk内核启动ok。问题是咱也不知道咋验证是不是wrk内核，看不出来区别。

#### 看到的东西

- 上交有门课是[Windows操作系统原理][1]，用到了WRK配套资料
- 有个[《Windows Internals COMPLETE》][2]的12小时视频，是对应的培训资料。现在找不到了，2013年1月24日archive.org记录网页上说那些内容已经过时了
- 另有一个《The Sysinternals Video Library》的12小时视频，可以在youtube上看，我看到@markrussinovich的这条[Twitter][3]说他们给公开了
- 国内一帮这个领域的名人们原来有个合辑[《深入Windows操作系统的内部工作原理》][4]，也是视频的

[1]: http://cse.sjtu.edu.cn/Windows/%E6%95%99%E5%AD%A6%E8%B5%84%E6%96%99.htm
[2]: https://web.archive.org/web/20130124114239/http://www.solsem.com/videos.html
[3]: https://twitter.com/markrussinovich/status/1099016584708923404
[4]: http://www.voidcn.com/article/p-yklrgaku-bba.html

