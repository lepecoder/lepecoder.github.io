---
title: Maximum-SubsequenceSum
date: 2017-10-02T15:35:00
tags: ['算法']
categories: 算法
---

### 题目

[https://pintia.cn/problem-sets/900290821590183936/problems/900291257604861953](https://pintia.cn/problem-sets/900290821590183936/problems/900291257604861953)


给出一段数列，求数列的最大子列和，并输出子列和的首尾元素。例如给出序列{ -2, 11, -4, 13, -5, -2 },最大的子列是{11,-4,13},应当输出{20,11,13}。如果有多个子列和相同，输出索引最小的那个。



如果最大子列和是负的则认为是0.



Sample Input:

```
10

-10 1 2 3 4 -5 -23 3 7 -21

```



Sample Output:

```

10 1 4

```



### 分析

使用线性搜索找到最大子列和，当更新最大值的时候顺便更新left和right。当子列和是负的时候，更新起始位置到临时变量t



### AC代码

```cpp

#include "bits/stdc++.h"

using namespace std;

int main(int argc, char const *argv[])

{

    int k, i, a[10010];

    cin >> k;

    for(i = 0; i<k; i++) cin >> a[i];

    int mmax = 0, sum = 0;

    int t=k, l=0, r=k;

    for(i=k-1;i>=0;i--){

        sum += a[i];

        if(sum>=mmax){

            mmax = sum;

            l = i;

            r = t;

        }

        if(sum <= 0){

            sum = 0;

            t = i;

        }

    }

    cout << mmax << ' ' << a[l] << ' ' << a[r-1];



    return 0;

}



```
    