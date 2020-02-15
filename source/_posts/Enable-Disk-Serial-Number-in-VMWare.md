---
title: Enable Disk Serial Number in VMWare
date: 2019-12-24 22:34:30
tags: [VMWare, vmx]
---

分析的软件没有按照预期执行，对比了下，最终发现了问题出在DeviceIoControl上，虚拟机返回的值是个空值，进而查明它做的是一种anti-VM的检查。
实际上的逻辑是检查第一块硬盘的序列号，如果长度小于5则放弃Hook（判定为VM）。
搜了下找到了在vmx中加入disk.enableUUID="true"的配置即可让虚拟机中的虚拟硬盘有序列号（默认是空的），后面用脚本计算了.ini文件中应填的sn，运行成功。 
顺道接触了wmic命令,可以检查不少东西,wmi的一个cli接口.
