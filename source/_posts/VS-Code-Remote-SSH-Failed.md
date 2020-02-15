---
title: VS Code Remote SSH 失败
date: 2020-02-11 16:18:10
tags: [SSH]
---

VS Code的错误

```
> 
[16:04:58.813] > getsockname failed: Not a socket
> 
[16:04:58.823] > packet_write_poll: Connection to UNKNOWN port -1: Permission denied
> 过程试图写入的管道不存在。
> 
[16:04:59.269] "install" terminal command done
[16:04:59.270] Install terminal quit with output: 过程试图写入的管道不存在。
[16:04:59.270] Received install output: 过程试图写入的管道不存在。
[16:04:59.271] Stopped parsing output early. Remaining text: 过程试图写入的管道不存在。
[16:04:59.271] Failed to parse remote port from server output
[16:04:59.271] Resolver error: 
```

使用ssh命令在cmd窗口下得到的错误是：

```
C:\>ssh sh
getsockname failed: Not a socket
ssh_dispatch_run_fatal: Connection to UNKNOWN port -1: incomplete message
```

参考这个[评论][1]，注释掉.ssh/config里面的 ControlPath 就可以了


[1]: https://github.com/microsoft/vscode-remote-release/issues/629#issuecomment-503650294
