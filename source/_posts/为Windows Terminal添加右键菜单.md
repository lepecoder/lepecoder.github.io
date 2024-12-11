---
title: 为Windows Terminal添加右键菜单
date: 2020-06-25T19:32:00
tags:
categories:
---

# open Windows terminal here

> 为Windows Terminal添加右键菜单。

##### Step 1: 下载图标

在github repo里下载[Terminal的图标](https://github.com/yanglr/WindowsDevTools/blob/master/awosomeTerminal/icons/wt_32.ico)，以显示在右键菜单中。

保存到如下位置【修改userName为你的用户名】

```
C:\Users\[userName]\AppData\Local\Microsoft\WindowsApps
```

##### Step 2: 增加注册表信息

保存如下内容到`wt.reg`，注意修改userName为你的用户名

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
@="Windows terminal here"
"Icon"="C:\\Users\\[userName]\\AppData\\Local\\Microsoft\\WindowsApps\\wt_32.ico"

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
@="C:\\Users\\[userName]\\AppData\\Local\\Microsoft\\WindowsApps\\wt.exe"
```

执行注册表文件



##### 效果

![image-20200625193036019](https://img2020.cnblogs.com/blog/1205530/202006/1205530-20200625193058645-607924601.png)

    