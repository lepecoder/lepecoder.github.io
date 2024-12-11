---
title: hdu-1171 Big Event in HDU
date: 2017-08-30T17:38:00
tags:
categories:
---

### 题目
[hdu-1171](http://acm.hdu.edu.cn/showproblem.php?pid=1171)
大意是有n类设备,每类设备的价值相同,要求将所有的设备分成价值尽量相等的两份.
### 分析
这是一个[多重背包](http://www.cnblogs.com/lepecoder/p/bag-3.html)问题,而背包的容量就是价值的一半

多重背包的一个解法就是转化为01背包,将背包的数量属性直接反映为m个价值相等的01背包物品

### AC代码
```cpp
#include "bits/stdc++.h"
using namespace std;
int main(int argc, char const *argv[])
{
    int n, a[1010], dp[1010], v, i, x, y;
    while(cin >> n && n!=-1){
        int k=0, sum=0;
        for(i = 0; i<n; i++){
            cin >> x >> y;
            while(y--){
                a[k++] = x;
                sum+=x;
            }
        }
        int V = sum / 2;
        memset(dp, 0, sizeof(dp));
        for(i=0;i<k;i++){
            for(v=V;v>=a[i];v--){//
                dp[v] = max(dp[v], dp[v-a[i]]+a[i]);
            }
        }
        cout << sum - dp[V] << ' ' << dp[V] << endl;
    }
    return 0;
}

```
    