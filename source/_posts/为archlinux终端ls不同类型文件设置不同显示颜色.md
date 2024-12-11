---
title: 为archlinux终端ls不同类型文件设置不同显示颜色
date: 2017-11-16T16:09:00
tags:
categories:
---

---
title: 为archlinux终端ls不同类型文件设置不同显示颜色
date: 2017-11-13 20:53:55
tags: linux
categories: linux

---
archlinux终端默认所有文件都以同样的颜色显示，这样不容易区分文件和目录，可以通过在`.bashrc`添加
```bash
alias ls='ls --color=auto'
```
这样使用`ls`列出目录时默认就是显示颜色的模式。

如果颜色设置不能让你满意，也可以自己设置某些文件的颜色。
系统默认的颜色设置在`~/.dir_colors`，可以通过修改这里的颜色设置调整显示效果，颜色的配置是文件类型+显示效果，其中文件类型有
```plain
	  no 　　　NORMAL, NORM 全局默认
      fi　　　　FILE 普通文件
      di 　　　 DIR 目录
      ln　　　　SYMLINK, LINK, LNK 链接
      pi　　　　FIFO, PIPE 管道
      do　　　　DOOR Door
      bd　　　　BLOCK, BLK 块设备
      cd　　　　CHAR, CHR 字符设备
      or　　　　ORPHAN 目标不存在到符号链接
      so　　　　SOCK 套接字Socket
      su　　　　SETUID 属主setuid有效的文件
      sg　　　　SETGID 属组setuid有效到文件
      tw　　　　STICKY_OTHER_WRITABLE Directory that issticky and other-writable ( t,o w)
      ow　　　　OTHER_WRITABLE Directory that isother-writable (o w) and not sticky
      st　　　　STICKY Directory with the sticky bit set (t) and not other-writable
      ex　　　　EXEC Executable file (i.e. has ‘x’ set inpermissions)
      mi　　　　MISSING Non-existent file pointed to by asymbolic link (visible when you type ls -l)
      lc　　　　 LEFTCODE, LEFT Opening terminalcode
      rc 　　　　RIGHTCODE, RIGHT Closing terminalcode
      ec　　　　ENDCODE, END Non-filenametext   
```

显示效果有：
```plain
00 　　　　默认
01 　　　　加粗
04 　　　　下划线
05　　　　闪烁
07 　　　　反显
08 　　　　隐藏

31～37　　　　分别表示前景色为红、绿、橙、蓝、紫、青、灰
90～97　　　　分别表示前景色为深灰、淡红、淡绿、黄色、淡蓝、淡紫、青绿、白色
40～47　　　　分别表示背景色为黑、红、绿、橙、蓝、紫、青、灰
100～106　　　分别表示背景色为深灰、淡红、淡绿、黄色、淡蓝、淡紫、青绿
```

在vim里可以直接预览颜色
![](http://osxdn70ll.bkt.clouddn.com/17-11-13/26099138.jpg)
    