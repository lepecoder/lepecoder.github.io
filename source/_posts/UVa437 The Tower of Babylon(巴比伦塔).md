---
title: UVa437 The Tower of Babylon(巴比伦塔)
date: 2017-08-19T20:35:00
tags: ['动态规划']
categories: 算法
---

### 题目

有n(n<=30)种立方体,每种有无穷多个,摞成尽量高的柱子,要求上面的立方体要严格小于下面的立方体.

[原题链接](https://vjudge.net/problem/UVA-437)



### 分析

顶面的大小会影响后续的决策,但不能直接用`d[a][b]`来表示,因为可能有多个立方体长宽相等,因此用`d[i][j](i<n,j<3)`表示第i个立方体的第j条边,为了方便比较大小,边按照从小大到排序.

每增加一个立方体后顶面的长宽都会严格减小,因此是一个DAG图上的最长路问题.



### AC代码

```cpp

#include "bits/stdc++.h"

using namespace std;

const int maxn = 35;

int n;

int blocks[maxn][3];    //存储第maxn个方块的3条边,且边是有序的

int d[maxn][3];



//得到第b个方块除了dim的两个高度

void get_dimensions(int *v, int b, int dim){

    int idx = 0;

    for(int i=0;i<3;i++){

        if(i != dim)

            v[idx++] = blocks[b][i];

    }

}



//在第i个方块上,以第j个边为高最多可以达到的高度

int dp(int i, int j){

    if(d[i][j]>0) return d[i][j];

    d[i][j]=0;

    int v1[2], v2[2];

    get_dimensions(v1, i, j);

    for(int a=0;a<n;a++){

        for(int b=0;b<3;b++){

            get_dimensions(v2, a, b);

            if(v2[0]<v1[0] && v2[1]<v1[1])  //v2可以放到v1上面

                d[i][j]=max(d[i][j], dp(a,b));  //

        }

    }

    d[i][j]+=blocks[i][j];  //加上本身的高度

    return d[i][j];

}



int main(int argc, char const *argv[])

{

    int kase = 0, i, j;

    while(cin >> n && n){

        for(i=0;i<n;i++){

            for(j=0;j<3;j++){

                cin >> blocks[i][j];

            }

            sort(blocks[i], blocks[i]+3);

        }



        // cout << "q1";

        memset(d, 0, sizeof(d));

        int ans = 0;

        for(i=0;i<n;i++){

            for(j=0;j<3;j++){

                ans = max(ans, dp(i,j));

            }

        }

        cout << "Case " << ++kase << ": maximum height = " << ans << endl;

    }

    return 0;

}



```
    