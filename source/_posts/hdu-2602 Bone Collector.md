---
title: hdu-2602 Bone Collector
date: 2017-08-31T17:04:00
tags:
categories:
---

### 题目
[http://acm.hdu.edu.cn/showproblem.php?pid=2602](http://acm.hdu.edu.cn/showproblem.php?pid=2602)

### 分析
基础背包问题,有一个容量为V的背包,各种骨头有大小和价值两种属性,求背包能装的骨头的最大价值.

### AC代码
```cpp
#include "bits/stdc++.h"
using namespace std;
int val[1010], vol[1010], dp[1010];
int main(int argc, char const *argv[])
{
    int T, N, V, i, v;
    cin >> T;
    while(T--){
        cin >> N >> V;
        for(i = 0; i<N; i++)
            cin >> val[i];
        for(i = 0; i<N; i++)
            cin >> vol[i];
        memset(dp,0,sizeof(dp));
        for(i=0;i<N;i++){
            for(v=V;v>=vol[i];v--){
                dp[v] = max(dp[v], dp[v-vol[i]]+val[i]);
            }
        }
        cout << dp[V] << endl;
    }
    return 0;
}
```
    