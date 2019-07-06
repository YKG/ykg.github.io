---
title: 配置PHP本地开发环境
date: 2019-07-06 20:13:34
tags: [PHP]
---

昨天使用mutagen把文件同步问题解决了，今天来给xdebug配上，和IDE配合使用，避免使用printf/dump形式来调试程序。

配了半天也没弄好，后来似乎可以又影响了，但在phpstorm里面根本无法检查变量，原因未知，只好切换到vagrant。

在ubuntu上装php还是费了很大劲，装的这个也没有使用之前docker那种的`fastcgi_pass: 127.0.0.1:9000`，而是一个unix sock，nginx.conf让只能选一个

```conf
# With php-fpm (or other unix sockets):
fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
# With php-cgi (or other tcp sockets):
#fastcgi_pass 127.0.0.1:9000;                       # docker上的长这样
```

安装过程是根据这个[wizard][1]指导的，把phpinfo()的网页输出全选复制粘贴到textarea里面，自动识别配置，让后给出安装建议。

但这个只是做到了把extension的so编译出来并放到合适的位置了，还需要自己去配置开启，xdebug的ini还是有很多要加

```bash
sudo vi /etc/php/7.2/fpm/php.ini
```

添加

```ini
[Xdebug]
zend_extension = /usr/lib/php/20170718/xdebug.so
xdebug.remote_enable = 1
xdebug.remote_connect_back = 1
xdebug.remote_port = 9001       # dbg信息转发到remote_host:remote_port上
xdebug.max_nesting_level = 512
xdebug.remote_host="10.21.39.50" # host主机ip
xdebug.idekey = "PHPSTORM"
xdebug.default_enable = 1
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_handler="dbgp"
```

晚上才明白过来，那个`remote_port`是说的host机器上的端口，这也就解释了为什么在vm里面查看listen端口见不到9001，反而在host机器上看有listen。PhpStorm那个ZeroConfig的remote debug就是默认开启监听9000，然后xdebug根据这个ini配置来知道要把dbg信息发送到哪里。

我之前一直以为这个9000是为了在vm中机器开放listen，然后由phpstorm去连接呢。之前因为docker中的php-fpm监听9000，所以还想着要把端口改掉，免得冲突，当时就曾发现没改的时候竟然phpstorm哪边也可以让程序中断执行，虽然并不能evaluate表达式，但确实证明下断生效了，明白了上面的过程，就好解释了。

此外看到文档提到remote_connect_back设置的时候，remote_host是无效的，不知道怎么理解。


[1]: https://xdebug.org/wizard.php
