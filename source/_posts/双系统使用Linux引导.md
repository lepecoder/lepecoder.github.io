---
title: 双系统使用Linux引导
date: 2019-11-07T20:23:00
tags:
categories:
---

今天在装linux的window双系统时，出现在无法使用linux引导的问题，开机总是自动进windows，照理来说我先装的window，后装的linux，应该是开机进grub引导才对。在主板的boot里根本没有linux项，后来用EasyUEFI检查发现linux的启动项被禁止和隐藏了！！

后来查了查，部分品牌的电脑会出现无法使用linux引导的问题，BIOS的boot里只有windows的启动项。如果是传统的引导方式，可以通过EasyBCD在windows系统下添加linux启动项，而UEFI的启动方式就没法用window引导linux了。

此时可以使用linux的引导文件替换windows的引导文件，让主板以为你这是windows的引导项。

虽然通过正常的方式无法进入linux系统，但是可以通过linux的启动盘找到linux的启动项，我用的是manjaro系统，插入刻录好的U盘，选择USB方式引导，在Detect EFI bootloaders里可以找到安装好的manjaro系统的引导项。通过它可以进入Linux系统。

![](https://img2018.cnblogs.com/blog/1205530/201911/1205530-20191107202144487-1238497231.jpg)



进入系统的`/boot/efi/EFI/`目录可以看到如下三个文件夹

```shell
总用量 32
drwx------ 2 root root 8192 10月  7 05:53 Boot
drwx------ 2 root root 8192 10月  7 14:14 Manjaro
drwx------ 4 root root 8192 10月  7 15:07 Microsoft
```

其中Manjaro里的grubx64.efi就是Linux的bootloader。

为了开机使用Linux引导我们可以使用Manjaro的grubx64.efi替换Microsoft的bootmgfw.efi，在我电脑上的具体命令是：

```shell
# 备份原文件
cp Microsoft/Boot/bootmgfw.efi Microsoft/Boot/bootmgfw1.efi

# 替换
cp Manjaro/grubx64.efi Microsoft/Boot/bootmgfw.efi
```

但这么以来即使你在Linux的grub里选择Windows Boot Manager项也不能进入Windows了，因为这个启动项指向的Windows bootloader已经被Linux替换了，所以还需要修改grub的Windows启动项，让它指向我们备份的bootmgfw1.efi。efi文件是不可读的，我们可以修改的是cfg文件。

看一个当前目录的Boot文件夹，里面有一个叫grub.cfg的文件，使用cat命令查看内容：

```shell
cat Boot/grub.cfg

search.fs_uuid 766eca58-fb60-457a-b79c-607f2c728407 root hd1,gpt4
set prefix=($root)'/boot/grub'
configfile $prefix/grub.cfg
```

发现它实际上是引用的`/boot/grub/grub.cfg`的内容，使用vim打开它，搜索`Windows`可以发现Windows的启动项

```shell
menuentry 'Windows Boot Manager (on /dev/sdb1)' --class windows --class os $menuentry_id_option 'osprober-efi-000E-17EB' {
	savedefault
	insmod part_gpt
	insmod fat
	set root='hd1,gpt1'
	if [ x$feature_platform_search_hint = xy ]; then
	  search --no-floppy --fs-uuid --set=root --hint-ieee1275='ieee1275//disk@0,gpt1' --hint-bios=hd1,gpt1 --hint-efi=hd1,gpt1 --hint-baremetal=ahci1,gpt1  000E-17EB
	else
	  search --no-floppy --fs-uuid --set=root 000E-17EB
	fi
	chainloader /EFI/Microsoft/Boot/bootmgfw.efi
}
```

将chainloader中的bootmgfw.efi改成bootmgfw1.efi

重启应该就可以了。
    