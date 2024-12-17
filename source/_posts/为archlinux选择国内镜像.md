---
title: 为archlinux选择国内镜像
date: 2017-07-26T11:48:00
tags:
categories:
---

> archlinux采用滚动更新,需要经常更新系统,因此一个快速且足够新的镜像就很重要了



## 获得镜像列表

选择镜像主要考虑速度和状态两方面,速度快的镜像可以让你更快的更新系统,状态新的镜像可以是你的系统一直保持最新状态.



如果你想知道知道中国大陆都有哪些镜像站可以访问[Pacman镜像列表生成器](https://www.archlinux.org/mirrorlist/)或是直接下载官方镜像列表,pacman的配置文件在`/etc/pacman.d/mirrorlist`



```

wget -O /etc/pacman.d/mirrorlist https://www.archlinux.org/mirrorlist/all/

#或是

wget -O /etc/pacman.d/mirrorlist https://www.archlinux.org/mirrorlist/?country=CN

```



许多镜像站没在官方的列表里,可以手动添加到你的镜像列表里,但这些镜像有可能很长时间没有更新了,可以从[这里](https://www.archlinux.org/mirrors/status/)检查镜像状态.



- 电信

    - http://mirror.bit.edu.cn/archlinux/ - 北京理工大学

    - http://mirrors.aliyun.com/archlinux/ - 阿里巴巴

- 联通

    - http://mirrors.sohu.com/archlinux/

    - http://mirrors.yun-idc.com/archlinux/

- 教育网

    - http://ftp.sjtu.edu.cn/archlinux/ - 上海交通大学y

    - http://mirrors.4.tuna.tsinghua.edu.cn/archlinux/ (ipv4 only)

    - http://mirrors.6.tuna.tsinghua.edu.cn/archlinux/ (ipv6 only)

    - http://mirror.lzu.edu.cn/archlinux/ - 兰州大学

    - https://mirrors.nju.edu.cn/archlinux/ - 南京大学



## 启用镜像



取消你想启用的镜像前的注释

注意,使用http协议的比使用ftp的更快

刷新镜像列表



```

pacman -Syyu

```



要注意镜像并不是越多越好,pacman默认只会连接第一个镜像地址,除非第一个不可用才会尝试链接第二个.



因此可以尝试将镜像按速度排序



## 将镜像按速度排序



- 使用`rankmirrors`

备份现在的镜像文件



```

cp mirrorlist mirrorlist.backup

```



使用rankmirrors将mirrorlist.back里的镜像按速度排序,找出前6个放到镜像文件里



```

rankmirrors -n 6 mirrorlist.backup > mirrorlist

```



- 使用reflector

直接把最近同步的镜像按速度排序覆盖 `/etc/pacman.d/mirrorlist`



```

reflector --verbose -l 200 -p http --sort rate --save /etc/pacman.d/mirrorlist

```

因此要一个个测试连接速度,可以比较慢,耐心等就好
    