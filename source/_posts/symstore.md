---
title: symstore
date: 2019-12-16 12:40:46
tags: [Windows, Symbols]
---

将`c:\SymbolsWin2003`下面所有的`*.pdb`文件索引到`c:\Symbols`中，`c:\Symbols`可以作为cache用于调试时符号解析。处理后的结果和直接与微软官方的符号服务器结构类似（等同，symstore会生成对应hash目录）。

```
symstore add /r /p /f \\localhost\c$\SymbolsWin2003\*.* /s c:\Symbols /t "Windows Server 2003 Standard" /v "x86 free" /c "ykg add"
```

