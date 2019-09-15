---
title: autosub语音转文字（自动生成字幕）
date: 2019-09-15 11:36:37
tags: [autosub, 音频]
---

昨天看圆桌派，马家辉讲到一句“不是不好，是不够”，正好前几天把微信签名由“HARD CORE”改成了"NOT ENOUGH"，不记得在哪里看到这一句的，印象里是李自然说，但不想过一遍视频来找，想转成文字搜索，于是从[知乎][1]知道了"讯飞听见"和"autosub"。讯飞只有1小时免费。转了文字也没搜到。花了点时间搜公众号，也没找到，就此打住。

autosub是使用Google的语音识别，在mac上需要现状ffmpeg，只看到ffmpeg官网有下载，没有安装指南。转而到服务器上使用，`pip3 install autosub`之后执行`autosub`提示找不到命令，但看着pip命令也执行成功了，不知道安装目录在哪里，搜了一会儿才知道是在`~/.local/bin`里，执行又有

```
ykg@proxy:~$ autosub lzr.mp3
  File "/home/ykg/.local/bin/autosub", line 136
    print "The given file does not exist: {0}".format(filename)
                                             ^
SyntaxError: invalid syntax
```

因为怕老的python不支持，所以使用的pip3安装的，结果就是上面，看github上有人说要2.7，遂都pip uninstall了，用pip重新安装，设置完PATH就可以执行了


[1]: https://www.zhihu.com/question/20124290
