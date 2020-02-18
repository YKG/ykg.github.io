---
title: remove already tracked big files in git history
date: 2020-02-18 22:17:37
tags: [Git]
---

参考这两个连接：

- [彻底删除git中的较大文件（包括历史提交记录）][1]
- [Git如何永久删除文件(包括历史记录)][2]



看大文件列表

```bash
git rev-list --all | xargs -rL1 git ls-tree -r --long | sort -uk3 | sort -rnk4 | head -10
```

执行删除

```bash
#文件的路径 {filepath}
git filter-branch --force --tree-filter "rm -f {filepath}" -- --all
```

参考执行结果：
```bash
$ git rev-list --all | xargs -rL1 git ls-tree -r --long | sort -uk3 | sort -rnk4 | head -20
100644 blob 52f283625cb29f829babefb624973fdc0c16324d 14560728   src/mrapps/rtiming.so
100644 blob f95df5da114004ebfcb783b456e34b0344069f63 14560632   src/mrapps/mtiming.so
100644 blob d8ea8b98a481442b20c486613a67d3274395aa26 14539232   src/mrapps/crash.so
100644 blob be9922e766ca528d4e943c95a9542d9c70a50676 14538336   src/mrapps/indexer.so
100644 blob 1a689a44255410ffac88b666fea17c9c34f9f3e0 14538184   src/mrapps/nocrash.so
100644 blob c88f311848b42519c1eb0e895ee72d6468648bd7 14534216   src/main/wc.so
100644 blob 31f4832d6d30bffb2421d6fce44cc79936bf611c 14534216   src/mrapps/wc.so
100644 blob 247f4dfde6d6fe0546c7090a4208be330acb1c56 11980400   src/main/mrsequential
100644 blob abddfbd5e64c42e12f0fd91793ab7bb590cd279f 11970000   src/main/mrworker
100644 blob 3367f486c7652de177c2121822690f860b3a217e 10364562   src/main/mrmaster
100644 blob d39524d8f3af18c12c674d94e0016e1aecb29c9a  594262    src/main/pg-huckleberry_finn.txt
100644 blob 8a6d3f402f6fe061f513c4c236045f37ae0ac586  581863    src/main/pg-sherlock_holmes.txt
100644 blob 66e5304d6acf475ab9ba80b89bbc6307d1af5489  540174    src/main/pg-grimm.txt
100644 blob 1c5880c834205d2cdcda1ddce3c746a547fc8eb2  453168    src/main/pg-dorian_gray.txt
100644 blob 6f5f2c9f91a70f5bfe37e8fef94099847be7021c  441033    src/main/pg-frankenstein.txt
100644 blob f21b2a14cfc7fa128bde07f996fb15d748ab2c92  412665    src/main/pg-tom_sawyer.txt
100644 blob b6a376efaf4e6664dec67f8e5a69cdd00a6bb74d  228830    src/main/mr-tmp/mr-correct-wc.txt
100644 blob 9270f3dac6f941109710d512ac9910522b4a1b81  139054    src/main/pg-metamorphosis.txt
100644 blob da6dffb1792a406e80d0f0ae166d71f7426a2364  138885    src/main/pg-being_ernest.txt
100644 blob 6696e8f4031cc73be4e5490675b53c95943a0b07   20633    src/raft/test_test.go

$ git filter-branch -f --tree-filter "rm -f src/mrapps/mtiming.so" -- --all
WARNING: git-filter-branch has a glut of gotchas generating mangled history
         rewrites.  Hit Ctrl-C before proceeding to abort, then use an
         alternative filtering tool such as 'git filter-repo'
         (https://github.com/newren/git-filter-repo/) instead.  See the
         filter-branch manual page for more details; to squelch this warning,
         set FILTER_BRANCH_SQUELCH_WARNING=1.
Proceeding with filter-branch...

Rewrite d7438ea996c544923c78732d446ef85cff71912a (4/4) (6 seconds passed, remaining 0 predicted)
Ref 'refs/heads/master' was rewritten
```


[1]: https://blog.csdn.net/HappyRocking/article/details/89313501
[2]: https://www.cnblogs.com/shines77/p/3460274.html