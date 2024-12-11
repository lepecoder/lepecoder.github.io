---
title: Linux开机自动挂载Windows分区
date: 2017-11-16T16:07:00
tags:
categories:
---

---
title: Linux开机自动挂载Windows分区
date: 2017-11-08 22:09:28
tags: linux
categories: linux

---
教程以archlinux为例。

### 查找分区的UUID或LABEL

使用`blkid`查看分区的label和uuid信息。
```bash
sudo blkid
```
返回可能类似下面这样：

![](http://osxdn70ll.bkt.clouddn.com/17-11-9/61194385.jpg)

如果你想得到关于分区大小的信息，可以运行
```
sudo fdisk --list
```

### 修改自动挂载文件
```
sudo vim /etc/fstab
```
你可以使用`LABEL`或`UUID`来定位你要挂载的分区。
使用`man fstab`获取文件的详细配置信息。
下面是我的`fstab`内容：
```
UUID=0008-1C51                            /boot/efi      vfat    defaults,noatime 0 2
UUID=44a05151-a639-4e64-82ea-320085e7d1ba /              ext4    defaults,noatime,discard 0 1
tmpfs                                     /tmp           tmpfs   defaults,noatime,mode=1777 0 0
LABEL=OS								  /home/lxp/OS	 ntfs    defaults,noatime 0 0
LABEL=SSD								  /home/lxp/SSD  ntfs    defaults,noatime 0 0
UUID=FCF2FA11F2F9D03E					 /home/lxp/E ntfs  defaults,noatime 0 0
UUID=000FBF060008A44E					/home/lxp/F  ntfs    defaults,noatime 0 0
UUID=000DB56E000E40C4                  /home/lxp/G   ntfs     defaults,noatime 0 0
```
    