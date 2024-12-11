---
title: 04-树6 Complete Binary Search Tree（30 分）
date: 2017-11-12T20:47:00
tags:
categories:
---

---
title: 04-树6 Complete Binary Search Tree（30 分）
date: 2017-11-12 14:20:46
tags:
    - 完全二叉树
    - 二叉搜索树
categories: 数据结构

---

### [题目链接](https://pintia.cn/problem-sets/900290821590183936/problems/911438665550655488)

![](http://osxdn70ll.bkt.clouddn.com/17-11-12/52465835.jpg)

### 题目大意
给出n个节点,构造一棵完全二叉搜索树。
最后按层次遍历输出这棵树。

### 分析
搜索二叉树的中序遍历是按照由小到大的顺序遍历的。
而完全二叉树，知道树的结点后就可以唯一确定树的结构，因此知道树的结点数后，中序遍历并将结点由小到大给树赋值，就可以构建出树。

###AC代码
```cpp
#include <bits/stdc++.h>
using namespace std;
int b[1023], a[1023];
int j=0, N;
void inOrder(int x){
    if(x<=N){
        //遍历左子树
        inOrder(x + x);
        //更新根节点
        b[x] = a[j++];
        //遍历右子树
        inOrder(x+x+1);
    }
}

int main(int argc, char const *argv[])
{
    int i;
    cin >> N;
    for(i = 0; i<N; i++)
        cin >> a[i];
    sort(a, a + N);
    inOrder(1);
    cout << b[1];
    for(i = 2; i<=N; i++)
        cout << ' ' << b[i];

    return 0;
}

```
    