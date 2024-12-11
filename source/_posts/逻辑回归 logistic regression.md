---
title: 逻辑回归 logistic regression
date: 2020-04-15T17:27:00
tags:
categories:
---

﻿### 

逻辑回归其实并不“逻辑”也不是回归，而是分类模型，逻辑是指利用了Logistic非线性函数。

#### 逻辑回归

在logistic regression中，我们用logistic函数来预测类别标签的后验概率
$$
p(y=1|x)=\sigma(w^T \cdot x)=\frac{1}{1+exp(-w^T\cdot x)} \tag{1}
$$
这里$x=[x_1,\cdots,x_D,1]$和$w=[w_1,\cdots,w_D,b]$，是$D+1$维的增广的特征向量和权重向量。

标签$y=0$的后验概率
$$
p(y=0|x)=1-p(y=1|x) \tag{2}
$$
由公式1可以得到
$$
w^Tx=\log \frac{p(y=1|x)}{p(y=0|x)} \tag{3}
$$
其中$\frac{p(y=1|x)}{p(y=0|x)}$称为几率(Odds)，所以$w^Tx$就是几率的对数，所以逻辑回归也称为对数几率回归。

当正负样本概率相等时，$w^Tx=\log(1)=0$，因此逻辑回归的目的就是求参数$w$使得正样本$w^Tx>0$负样本$w^Tx<0$。



#### 损失函数

使用交叉熵损失函数，在二分类问题中
$$
J(w) = -\frac{1}{m}\sum_{i=1}^m\left(y^i\log(\hat{y}^i) + (1-y^i)\log(1-\hat{y}^i ) \right)
$$
$y^i$是样本$x^i$的真实标签，$\hat{y}^i$是模型的预测标签，正样本的损失是$\log(\hat{y})$负样本的损失是$\log(1-\hat{y})$

我们的目标是最小化损失函数。

#### 参数学习

##### 随机梯度下降

SGD是通过对损失函数求偏导确定梯度方向，沿着梯度方向更新参数以最小化损失函数
$$
\frac{\partial J(w)}{\partial w} = -\frac{1}{N} \sum_{i=1}^N x^i(y^i-\hat{y}^i)
$$

$$
w_{t+1} \leftarrow w_t - \alpha \frac{\partial J(w)}{\partial w}
$$

###### 代码实现

```python
# 梯度下降法
for _ in range(500):
    # 利用逻辑回归做预测
    y0 = logistic(w,ex)
    # 计算当前的交叉熵损失
    ce = cross_entropy(y,y0)
#     print("第{}轮，cross_entropy = {}".format(_,ce))
    # 求偏导    
    partial = (np.sum(ex*(ey-y0),axis=0))/N
    # 更新参数
    w = w-alpha*partial
```

![img](https://img2020.cnblogs.com/blog/1205530/202004/1205530-20200415172535236-2047943914.png)



##### 牛顿法

牛顿法在求解方程$f(\theta)=0$的根时主要是根据泰勒展开式进行迭代求解，假设有初始近似解$x_k$，那么$f(x)$在点$x_k$处的泰勒展开式
$$
f(x)\approx f(x_k)+f'(x_k)(x-x_k)
$$
令$f(x)=0$求解得到$x_{k+1}$
$$
x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}
$$
牛顿法的几何解释如下图

![这里写图片描述](https://img2020.cnblogs.com/blog/1205530/202004/1205530-20200415172534775-761681118.png)

在逻辑回归中，损失函数的最小值在$J'(w)=0$处。用牛顿迭代法求参数
$$
w_{t+1} \leftarrow w_t - \frac{J'(w)}{J''(w)}
$$

    