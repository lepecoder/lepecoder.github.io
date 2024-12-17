---
title: linux下sublime输入中文
date: 2017-11-16T16:08:00
tags: ['sublime', 'Linux']
categories: 开发工具
---

archlinux系统



1. 下载文件

```

git clone https://github.com/lyfeyaj/sublime-text-imfix.git

```

2. 复制文件

```

cd ~/sublime-text-imfix

sudo cp ./lib/libsublime-imfix.so /opt/sublime_text/

sudo cp ./src/subl /usr/bin/

```

3. 运行

```

#直接运行

subl

#如果不行，先载入so文件再运行

LD_PRELOAD=./libsublime-imfix.so subl

```

如果能运行，那么应该已经可以用中文输入法了。

效果图如下：

![](http://osxdn70ll.bkt.clouddn.com/17-11-9/63266044.jpg)
    