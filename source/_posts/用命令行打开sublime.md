---
title: 用命令行打开sublime
date: 2017-10-06T02:59:00
tags:
categories:
---

在linux下装了linux后默认并不能通过运行命令的方式打开，这就让我们不能像vim一样可以通过

```
vim <fileName>
```
来打开文件。


不过我们可以通过把sublime的执行文件放到PATH目录下的方式实现用命令打开sublime

1. 找到sublime的安装目录，我的是`/opt/sublime_text`
2. 建立执行文件到`/usr/local/bin`的软连接
```
ln -s /opt/sublime_text/sublime_text /usr/local/bin/subl
```
这样在终端执行`subl <fileName>`就可以用sublime打开文件
    