---
title: x64dbg脚本初试
date: 2019-10-21 21:56:29
tags: [x64dbg, 逆向]
---

目的是为了知道怎么用x64dbgpy插件的使用方法。
脚本的资料很少，没看到有什么教程，在这个[youtube视频][1]上看到了最基础的操作，是官网的github的[readme给出][2]的。


### 安装

- 需要分别安装python2.7的32位和64位版本，分别装到不同的目录下，比如我把64bit的安装到`c:\Python27`，32bit的装到`c:\Python-x86`下
- 插件会自动将python的目录填充到`x64dbg\x32\x32dbg.ini`和`x64dbg\x64\x64dbg.ini`的配置文件中

### 遇到的问题

#### 执行x64dbg GUI上的Plugins | Editor_x64dbg | Editor 提示找不到

- 发现GitHub上有人提了这个[issue][3]
- 我是用的应该是新的release，估计是没修复成。没细看原因，重启问题不再，先回避了。

#### 按照上面的tutorial视频，在command输入栏输入print 'Hi'命令，并没有效果

- 不知道是什么问题，没有去寻找解决方案
- 尝试在plugin的Editor中写这个命令执行，执行OK

#### Editor中执行的命令，输出结果会重复输出两遍

- 有人提了[issue][4]
- 看了下面的回复，按照作者的更正，将问题修复了

#### 怎样实现x64dbg中的alloc命令

- 在[_scriptapi_memory.h][5]中找到了`RemoteAlloc`，尝试在脚本里运行，但都没有达到预期，用的`memory.RemoteAlloc(0x07D80000, 0x1000)`，检查`print (debug.RemoteAlloc)`确实是一个function
- 后来了解到`inspect.getsource`可以把源码打出来，于是把`inspect.getsource(memory.Read)`和`inspect.getsource(memory.Write)`找了出来，随后用`grep -rn Memory_Write .`在x64dbg的安装目录搜索到了原来不是`.h`中的那样，而是在`https://github.com/x64dbg/x64dbgpy/blob/v25/swig/x64dbgpy/pluginsdk/_scriptapi/memory.py#L11`中
- 至此知道了上面`swig/x64dbgpy/pluginsdk/_scriptapi`中的就是各个部件的api部分
- 实际上`RemoteAlloc`就是和`alloc`命令使用的同样的参数顺序

#### x64dbg的Fill/memset命令只支持单字节赋值，不能字符串赋值

- 使用py脚本的`memory.Write(0x07D80100, "abc")`或者`memory.Write(0x00466497, bytearray.fromhex('E9 55 16 00 00 90 90 90'))`

#### 使用两次`x64dbg.Run()`时，第二次期待会在断点处停下来，但没有停，还抛了异常`0x0EEDFADE`

- 不知道是否和x64dbg的线程模型相关，但这个算是基础功能，应该有正确的处理步骤，但我不知道
- 参考资料: [Thread-safety (GitHub issue)][6], [The x64dbg threading model][7]
- 午饭时看了下threading model的blog，似乎并没有用错，原因未知
- 因为看到了日志部分，异常的前一行时windows的防火墙av类的dll，于是尝试将`x64dbg`目录、debugee所在目录全都加入到病毒与安全防护的文件夹例外里了，再次运行脚本，x64dbg直接退出了。重新打开后，两次Run和手动操作一样，没有问题。应该是安全防护的问题




### 其他

- 在py脚本中可以使用`x64dbg.DbgCmdExec('alloc 1000,07D80000')`这种方式来执行原生命令



[1]: https://www.youtube.com/watch?v=rIFA6t1Z9Fc&feature=youtu.be
[2]: https://github.com/x64dbg/x64dbgpy/blame/v25/README.md#L9
[3]: https://github.com/techbliss/X64dbg_script_editor/issues/5
[4]: https://github.com/techbliss/X64dbg_script_editor/issues/6
[5]: https://github.com/x64dbg/x64dbgpy/blob/v25/pluginsdk/_scriptapi_memory.h#L13
[6]: https://github.com/x64dbg/x64dbgpy/issues/8
[7]: https://x64dbg.com/blog/2016/10/20/threading-model.html


