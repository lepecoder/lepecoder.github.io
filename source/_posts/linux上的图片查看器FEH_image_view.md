---
title: linux上的图片查看器FEH_image_view
date: 2018-05-25T14:13:00
tags: ['FEH', 'Linux']
categories: 工具
---

Linux上的图片查看器，


![](http://p1f1jwe7c.bkt.clouddn.com/18-5-24/76810644.jpg)

简单，没有多余功能，打开快速，体积小


### 在终端用feh



```bash

# 直接执行feh显示当前目录所有图片

feh



# 或者指定图片名

feh pic1 pic2 pic3



# 显示一个目录里所有图片

feh dirname



```



### 在文件管理器里用feh

你可以把图片打开方式设为feh，但这样只能打开你选中的图片

下面的脚本可以让你打开图片时先显示选中的图片，同时可以浏览目录里的其他图片



```bash

#!/bin/bash



shopt -s nullglob



if [[ ! -f $1 ]]; then

echo "$0: first argument is not a file" >&2

exit 1

fi



file=$(basename -- "$1")

dir=$(dirname -- "$1")

arr=()

shift



cd -- "$dir"



for i in *; do

[[ -f $i ]] || continue

arr+=("$i")

[[ $i == $file ]] && c=$((${#arr[@]} - 1))

done



exec feh "$@" -- "${arr[@]:c}" "${arr[@]:0:c}"



```



然后你可以把图片的打开方式设为这个脚本
    