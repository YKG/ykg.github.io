---
title: 8050800C Windows Update 错误
date: 2019-12-23 20:33:26
tags: [Windows]
---

参考这个[链接][1]和下面的评论:


Did you receive the 8050800C error from Windows update while trying to get the Windows Defender updates? I did with my Windows 7 Pro 64.  Lots of sites tell a lot of stupid things to try, things that are only there to make the site catch more Google hits to serve more ads. Nobody seems to test their stuff anymore. I did. And I have a working, tested solution.
Executive summary of fix:
1. Download the the Windows Defender update file manually (mpas-fe.exe, available from https://www.microsoft.com/en-us/wdsi/definitions )
2. Start Windows in safe mode
3. Run the downloaded exe in safe mode and reboot (or optionally run the exe with Windows Update Service disabled)
4. It’s fixed!


echnomage Awunes
SEPTEMBER 18, 2019 AT 8:17 PM
Thank you for your solution. You might consider mentioning the option to disable Windows Update service instead of safe mode in your short directions. Example:
Download the the Windows Defender update file manually (mpas-fe.exe, available from https://www.microsoft.com/en-us/wdsi/definitions )
Start Windows in safe mode OR disable Windows Update Service
Run the downloaded exe in safe mode and reboot OR with Windows Update service disabled and no reboot
It’s fixed!
REPLY



主要就是下载 mpas-fe.exe 安装了，就OK了
我是先禁用了update服务之后安装的，安装过程啥提示都没有。之后再启用了update服务。
再进行update安装时就是上面的绿色结果。

照上面评论的说，不禁用udpate服务也是没问题的。


[1]: https://paalijarvi.fi/tgblog/2019/04/09/how-to-fix-windows-update-error-8050800c-about-windows-defender-definition-update/
