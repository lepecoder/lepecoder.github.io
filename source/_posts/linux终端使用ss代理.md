---
title: Linux终端使用ss代理
date: 2017-11-16T16:09:00
tags: ['Linux']
categories: 工具
---

>系统为archlinux

### 先将ss代理转化为http代理

安装polipo

```
sudo pacman -S polipo
```


### 修改配置文件`/etc/polipo/config`

```
#设置本地代理地址
proxyAddress = "::0"

#设置ss代理地址和端口
socksParentProxy = "lcoalhost:1081"
socksProsyType = socks5
```


### 启动polipo

```
sudo systemctl start polipo
```

### 设置别名

`vim ~/.bashrc`

添加

```
alias hp="http_proxy=http://localhost:8123"
```

8123是polipo的默认端口


### 测试

```
hp curl ip.gs
```



效果如下：

![](http://osxdn70ll.bkt.clouddn.com/17-11-9/23458551.jpg)
    