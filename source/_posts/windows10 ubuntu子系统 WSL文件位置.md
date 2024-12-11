---
title: windows10 ubuntu子系统 WSL文件位置
date: 2019-09-21T17:06:00
tags:
categories:
---

windows10 的linux子系统（windows subsystem for linux)WSL 文件位置

以我的系统为例，WSL的root目录对应windows的：

```
C:\Users\xiaoPeng\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs
```

其中AppData文件夹默认是隐藏的，但你可以在路径里输入AppData进去。但实际上微软为我们提供了一个默认的变量可以直接指向WSL的目录，`wsl$` 你可以在运行(win+R)或资源管理器的路径里直接输入`\\wsl$`进入Ubuntu的目录

![image.png](https://i.loli.net/2019/09/21/OmCGYjrSHLElIcd.png)

知道WSL的网络路径后你可以直接将它添加到资源管理器的网络位置里

![image.png](https://i.loli.net/2019/09/21/Ru9m7Bx6DeFzqhO.png)
    