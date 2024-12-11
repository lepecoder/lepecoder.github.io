---
title: 为Windows terminal preview添加右键菜单
date: 2019-12-26T17:34:00
tags:
categories:
---

微软的新终端Windows Terminal preview可以在Windows10应用商店或是[github 上的Windows Terminal Preview的release页](https://github.com/microsoft/terminal/releases)下载，顺便把图标[terminal.ico](https://github.com/microsoft/terminal/blob/master/res/terminal.ico)也下载下来。图标可以新建个文件夹放到`C:\Users\[Username]\AppData\Local\terminal`



新建注册表文件比如`wt.reg`，内容如下：

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
@="Windows terminal here"
"Icon"="C:\\Users\\[Username]\\AppData\\Local\\terminal\\terminal.ico"

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
@="C:\\Users\\[Username]\\AppData\\Local\\Microsoft\\WindowsApps\\wt.exe"
```

其中`Username`改为你的用户名。双击运行写入注册表。



此时在文件夹里右键就可以看到新的windows terminal here项


![](https://img2018.cnblogs.com/blog/1205530/201912/1205530-20191226173149750-1102334198.png)

如果点击后的目录不是当前目录的话，在`setting`里增加一项`"startingDirectory" : "."`，如下图：


![](https://img2018.cnblogs.com/blog/1205530/201912/1205530-20191226173201935-1561279687.png)

![](https://img2018.cnblogs.com/blog/1205530/201912/1205530-20191226173226512-820683704.png)
    