---
title: 设置vim支持gbk
date: 2017-10-23T20:42:00
tags:
categories:
---

linux下的默认字符集是utf-8，但Windows下默认是GBK，如果我们在linux下打开Windows中的文件就很容乱码，可以通过下面的设置使vim支持GBK编码。

首先，确认你的系统中安装了GBK，可以通过`locale -a`命令查看
```bash
$ locale -a

C
en_US.utf8
POSIX
zh_CN
zh_CN.gb18030
zh_CN.gb2312
zh_CN.gbk
zh_CN.utf8

```

如果没有可以通过编辑`/etc/locale.gen`，添加`zh_CN.gbk`，然后用`locale-gen`命令生成locale。


最后编辑`.vimrc`文件，添加
```bash
set fileencodings=utf-8,gbk
```
再次打开vim，显示就正常了。
    