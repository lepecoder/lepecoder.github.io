---
title: Python实现图像直方图均衡化算法
date: 2018-06-12T17:21:00
tags:
categories:
---

---
title: "Python实现图像直方图均衡化算法"
date: 2018-06-12T17:10:48+08:00
tags: [""]
categories: ["python"]

---

### 效果图

![](http://p1f1jwe7c.bkt.clouddn.com/18-6-12/5590468.jpg)


### 代码

```python

#!/usr/bin/env python3
# coding=utf-8

import matplotlib.image as mpimg
from matplotlib import pyplot as plt
import sys
import numpy as np


def equalization(gray_value):
    """
    传入灰度值，对灰度值做均衡化，不需要返回，直接修改传入的参数
    :param gray_value:
    """
    # 统计灰度直方图
    gray = np.zeros(256)
    row, column = gray_value.shape
    for i in range(row):
        for j in range(column):
            gray[gray_value[i][j]] += 1

    # 计算灰度占比
    gray /= (row * column)
    # 显示灰度直方图
    plt.subplot(2, 2, 2)
    plt.plot(gray)

    cumsum = np.cumsum(gray)  # 计算累积和

    # 均衡化
    # equa_t[i]=j表示原灰度值i经过均衡化后转化为灰度值j
    # 255×累积和四舍五入为int型
    equa_t = np.array((255 * cumsum + 0.5)).astype(np.int32)
    # 统计均衡化后的灰度数量
    equa_gray = np.zeros(256)
    for i in range(256):
        equa_gray[equa_t[i]] += gray[i]
    # 显示均衡化后的直方图
    plt.subplot(2, 2, 4)
    plt.plot(equa_gray)
    # 对原灰度矩阵做均衡化
    for i in range(row):
        for j in range(column):
            gray_value[i][j] = equa_t[gray_value[i][j]]


def run(img_path):
    img_array = mpimg.imread(img_path)
    plt.subplot(2, 2, 1)
    plt.imshow(img_array)
    img_array *= 255
    img_array = img_array.astype(np.int32)
    equalization(img_array[:, :, 0])
    equalization(img_array[:, :, 1])
    equalization(img_array[:, :, 2])
    img_array = img_array.astype(np.float64)
    img_array /= 255
    plt.subplot(2, 2, 3)
    plt.imshow(img_array)


if __name__ == "__main__":
    if sys.argv.__len__() <= 1:
        png = input("请输入要处理的图片名:\n")
    else:
        png = sys.argv[1]
    run(png)
    plt.show()

```
    