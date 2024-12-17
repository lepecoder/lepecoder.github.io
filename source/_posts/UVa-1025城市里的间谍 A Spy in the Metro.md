---
title: UVa-1025城市里的间谍 A Spy in the Metro
date: 2017-08-17T16:38:00
tags: ['动态规划']
categories: 算法
---

### 原题

[城市里的间谍](https://vjudge.net/problem/UVA-1025)



### 分析

动态规划,`dp[i][j]`表示你在时刻i,车站j,最少还要等待的时间. 边界条件`d[T][n]=0` _已经到达_,其他`d[T][i]=inf`_不可达_.



在一个站点时,有以下三种决策:

1. 等一分钟

2. 搭乘往左开的车(前提是有)

3. 搭乘往右开的车



### AC代码



```cpp

#include "bits/stdc++.h"

using namespace std;



const int maxn = 50 + 5;

const int maxt = 200 + 5;

const int INF = 1000000000;



// has_train[t][i][0]表示时刻t，在车站i是否有往右开的火车

int t[maxn], has_train[maxt][maxn][2];

int dp[maxt][maxn];



int main() {

  int kase = 0, n, T;

  while(cin >> n && n) {

    cin >>T ;

    int M1, M2, d;

    for(int i = 1; i <= n-1; i++) cin >> t[i];



    // 预处理，计算has_train数组

    memset(has_train, 0, sizeof(has_train));

    cin >> M1;

    while(M1--) {

      cin >> d;//针对每一俩车,更新它到车站的时间

      for(int j = 1; j <= n-1; j++) {

        if(d <= T) has_train[d][j][0] = 1;

        d += t[j];

      }

    }

    cin >> M2;

    while(M2--) {

      cin >> d;

      for(int j = n-1; j >= 1; j--) {

        if(d <= T) has_train[d][j+1][1] = 1;

        d += t[j];

      }

    }



    // DP主过程

    for(int i = 1; i <= n-1; i++) dp[T][i] = INF; //不可达

    dp[T][n] = 0; //已达



    for(int i = T-1; i >= 0; i--)

      for(int j = 1; j <= n; j++) {

        dp[i][j] = dp[i+1][j] + 1; // 等待一个单位

        if(j < n && has_train[i][j][0] && i+t[j] <= T)

          dp[i][j] = min(dp[i][j], dp[i+t[j]][j+1]); // 右

        if(j > 1 && has_train[i][j][1] && i+t[j-1] <= T)

          dp[i][j] = min(dp[i][j], dp[i+t[j-1]][j-1]); // 左

      }



    // 输出

    cout << "Case Number " << ++kase << ": ";

    if(dp[0][1] >= INF) cout << "impossible\n";

    else cout << dp[0][1] << "\n";

  }

  return 0;

}



```
    