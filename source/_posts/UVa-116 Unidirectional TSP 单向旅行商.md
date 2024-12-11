---
title: UVa-116 Unidirectional TSP 单向旅行商
date: 2017-08-24T14:28:00
tags:
categories:
---

### 题目
[https://vjudge.net/problem/uva-116](https://vjudge.net/problem/uva-116)

### 分析

设`d[i][j]`为从`(i,j)`到最后一列的最小开销，则`d[i][j]=a[i][j]+max(d[i+1][j+1],d[i-1][j+1])`
参考[数字三角形](http://www.cnblogs.com/lepeCoder/p/dp_triangle.html),用逆推的方法,先确定最后一列`d[i][n-1]=a[i][n-1]`,再确定`n-2`列,此时`d[i][n-2] = a[i][n-2]+min(d[i][n-1],d[i-1][n-1],d[i+1][n-1])`
最终推出全部的`d[i][j]`后,第一列最小的d就是答案.

另外要求打印路径,因此建立一个数组`next1[i][j]`,保存结点i,j之后的结点.
在逆推的时候,如果`d[rows[k]][j+1]+a[i][j] < d[i][j]` 就更新`next1[i][j]=rows[k]`

### AC代码
```cpp
#include "bits/stdc++.h"
using namespace std;
#define inf 0x3f3f3f3f
int main(int argc, char const *argv[])
{
    ios::sync_with_stdio(false);
    int a[150][150], d[150][150], next1[150][150];
    int ans = inf, first = 0, m, n, i, j, k;
    while (cin >> m && m != - 1) {
        cin >> n;
        ans = inf;

        //d[i][j]表示从i, j开始走可以经过的最小整数和
        //因此可以知道最后一列d[i][n-1] = a[i][n-1]
        for(i=0;i<m;i++)
            for(j=0;j<n;j++)
                cin >> a[i][j];

        for (i = 0; i < m; i++)
            d[i][n - 1] = a[i][n - 1];  //初始化最后一列

        for (j = n - 2; j >= 0; j--) {
            for (i = 0; i < m; i++) {
                int rows[3] = {i, i - 1, i + 1};
                if (i == 0) rows[1] = m - 1; //第一行与最后一行相邻
                if (i == m - 1) rows[2] = 0;
                sort(rows, rows + 3);
                d[i][j] = inf;
                for (k = 0; k < 3; k++) {
                    int v = a[i][j] + d[rows[k]][j + 1];
                    if(v < d[i][j]){
                        d[i][j] = v;//更新最短路
                        next1[i][j] = rows[k];//记录路径,只记录行,因为列是递增的
                    }
                }
                if(j ==0 && d[i][j] < ans){
                    ans = d[i][j];
                    first = i;
                }
            }
        }
        //因为上面是从n - 2列开始, 所以只有一列是要单独处理
        if(n==1){
            for(i=0;i<m;i++){
                if(ans > a[i][0]){
                    ans = a[i][0];
                    first = i;
                }
            }
        }


        cout << first + 1;
        for(int i=next1[first][0], j=1; j<n; i = next1[i][j],j++){
            cout << ' ' << i+1;
        }
        cout << endl << ans << endl;
    }
    return 0;
}
```
    