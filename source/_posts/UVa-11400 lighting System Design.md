---
title: UVa-11400 lighting System Design
date: 2017-09-02T21:59:00
tags:
categories:
---

### 题目

[https://vjudge.net/problem/Uva-11400](https://vjudge.net/problem/Uva-11400)

设计一个照明系统,有n种灯泡,每种灯牌配备一个电源,灯泡有电压 电源价格 单价 数量 四种属性

1. 当一种灯泡的数量为0时,则可以省下一个电源
2. 可以用一个高电压的灯泡代替一个低电压的灯泡

设计一种灯泡替换方案,使得总费用最小

### 分析

只要存在一种灯泡,无论它的数量是1还是100都需要一个电源,因此,在我们替换灯泡时,如果替换1个可以省钱,那么替换所有必然更省钱,因此我们得到一个结论,每种电压的灯泡,要么全换,要么全不换

将单价高的灯泡换成单价低的灯泡必然是省钱的,但是将单价低的灯泡换成单价高的灯泡也有可能是省钱的,因为可以省下电源的费用,因此,要给出一种有效的灯泡替换方案是不容易的,但是我们通过动态规划找出所有可能的灯泡替换方案,从而得到费用最低的方案.



先按照电压从小到大排序,定义`d[i]`是前i个灯泡的最小开销,`s[i]`是前i种灯泡的数量,在考虑第i种灯泡的时候可以有`j < i` `d[i] = min(d[j]+(s[i]-s[j])*c[i]+k[i])`

`d[j]+(s[i]-s[j])*c[i]+k[i]`表示将j+1到i的灯泡替换第i种灯泡时的费用. __为什么是将j+1到i的灯泡全部替换为第i种灯泡而不是只替换其中的几种呢?__ 可以设想,假设将第j种灯泡替换为第i种灯泡可以得到更优的解,那么替换第j+1时也应当得到更优的解,否则在第j+1轮循环的时候就应该用j+1替换j了



最后`d[n]`就是本题的解了



### AC代码

```cpp

#include "bits/stdc++.h"

using namespace std;

#define maxn 1010

#define INF 0x3f3f3f3f

struct Node{

    int v, k, c, l;

    bool operator < (const Node& x) const{

        return v < x.v;

    }

}light[maxn];

int d[maxn], s[maxn], n, i, j;

int main(int argc, char const *argv[])

{

    while(cin >> n && n){



        for(i=1;i<=n;i++)

            cin >> light[i].v >> light[i].k >> light[i].c >> light[i].l;

        sort(light + 1, light + n + 1);

        s[0]=0;

        for(i = 1; i<=n; i++)

            s[i] = s[i - 1]+light[i].l;

        d[0]=0;

        for(i = 1; i<=n; i++){

            d[i]=INF;

            for(j = 0; j<i; j++){

                //d[i]是布置前i种灯泡的最小开销

                //下面的式子表示将第j+1到第i种全部更换为第i种灯泡

                d[i] = min(d[i], d[j]+(s[i]-s[j])*light[i].c+light[i].k);

            }

        }

        cout << d[n] << endl;

    }



    return 0;

}

```
    