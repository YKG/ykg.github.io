---
title: 怎样检查命令是否存在
date: 2019-06-04 21:11:19
tags: [Shell]
---

```shell
command -v <the_command> # the_command存在时$?为0，不存在时为1 
```

在shell中检查：
```shell
if [[ ! `command -v gdb >/dev/null 2>&1` ]]; then
    echo "skipping sse4.2 check - gdb is required"
    exit 0
fi
```


[参考1][1]
[TiKV PR #4832][2]

[1]: https://stackoverflow.com/questions/592620/how-to-check-if-a-program-exists-from-a-bash-script
[2]: https://github.com/tikv/tikv/pull/4832
