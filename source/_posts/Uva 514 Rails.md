---
title: Uva 514 Rails
date: 2018-02-07T21:52:00
tags:
categories:
---

---
title: "Uva 514 Rails"
date: 2018-02-07T21:38:33+08:00
tags: ["栈"]
categories: ["数据结构"]

---

### 题目
![](http://p1f1jwe7c.bkt.clouddn.com/18-2-7/44023371.jpg)

[原题链接](https://vjudge.net/problem/UVA-514)

### 分析

直接利用STL中的stack模仿栈的操作.

### 代码

```c++
#include "bits/stdc++.h"
using namespace std;
const int MAXN = 1010;

int main() {
    int n, target[MAXN];
    bool isN = false;
    while (1)
    {
        if (!isN) {
            cin >> n;
            if (n == 0)
                return 0;
            isN = true;
        }

        stack<int> s;
        int A = 1;  //1-n的数列
        int B = 1;  //输入数列指针

        for (int i = 1; i <= n; i++) {
            cin >> target[i];
            if (target[i] == 0) {
                break;
            }
        }
        if (target[1] == 0) {
            cout << endl;
            isN = false;
            continue;
        }

        int ok = 1;
        // 模仿栈的行为
        while (B <= n) {
            if (A == target[B]) {   //匹配
                A++; B++;
            }
            else if (!s.empty() && s.top() == target[B]) {  //栈顶元素等于目标元素,出栈
                s.pop(); B++;
            }
            else if (A <= n) {  //没有得到目标元素,继续入栈
                s.push(A++);
            }
            else {
                ok = 0;
                break;
            }
        }
        printf("%s\n", ok ? "Yes" : "No");
    }
    return 0;
}

```
    