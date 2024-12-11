---
title: Python实现图像边缘检测算法
date: 2018-06-12T17:16:00
tags:
categories:
---

---
title: "Python实现图像边缘检测算法"
date: 2018-06-12T17:06:53+08:00
tags: ["图形学"]
categories: ["python"]

---

### 实现效果

![](http://p1f1jwe7c.bkt.clouddn.com/18-6-12/5650040.jpg)

![](http://p1f1jwe7c.bkt.clouddn.com/18-6-12/48501449.jpg)

###代码

```python

#!/usr/bin/env python3
# coding=utf-8
from PIL import Image
import numpy as np


img_name = input("输入要处理的图片\n")
# img_name = "t3.png"
img = Image.open(img_name).convert("L")  # 读图片并转化为灰度图
img.show()
img_array = np.array(img)  # 转化为数组

w, h = img_array.shape

img_border = np.zeros((w-1, h-1))

for x in range(1, w - 1):
    for y in range(1, h - 1):
        Sx = img_array[x + 1][y - 1] + 2 * img_array[x + 1][y] + img_array[x + 1][y + 1] - \
                img_array[x - 1][y - 1] - 2 * \
                img_array[x - 1][y] - img_array[x - 1][y + 1]
        Sy = img_array[x - 1][y + 1] + 2 * img_array[x][y + 1] + img_array[x + 1][y + 1] - \
                img_array[x - 1][y - 1] - 2 * \
                img_array[x][y - 1] - img_array[x + 1][y - 1]
        img_border[x][y] = (Sx * Sx + Sy * Sy) ** 0.5

img2 = Image.fromarray(img_border)
img2.show()

```
    