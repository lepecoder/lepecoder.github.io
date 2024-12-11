---
title: 在Windows10中运行debug程序
date: 2017-08-13T16:40:00
tags:
categories:
---

1. 下载debug.exe
![](http://osxdn70ll.bkt.clouddn.com/17-8-13/84966749.jpg)
2. 下载DOSBox
![](http://osxdn70ll.bkt.clouddn.com/17-8-13/27521429.jpg)
3. 安装DOXBox,尽量不要装在C盘
4. 将debug.exe放到`F:/TASM`
5. 运行DOSBox.exe,执行
```
mount c f:\TASM  #挂载目录
c:
debug
```
![](http://osxdn70ll.bkt.clouddn.com/17-8-13/10052652.jpg)
    