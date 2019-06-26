---
title: 使用TCP端口测量网络延迟
date: 2019-06-26 08:26:46
tags: [Latency, TCP, psping, hping, mtr]
---

在Windows上由psping可以用，Mac上可以用hping，但不知怎么现在Mac上的hping不能用了，错误`Sorry, this hping binary was compiled without TCL scripting support`。

找替代，看了nmap，似乎没有。mtr以为只有icmp包，后来看到Linode上的这篇[mtr高级][1]，果然可以tcp

```bash
sudo mtr --tcp --port 22 hostname
```


[1]: https://www.linode.com/docs/networking/diagnostics/diagnosing-network-issues-with-mtr/#advanced-mtr-techniques
