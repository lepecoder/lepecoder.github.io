---
title: 03-树3 Tree Traversals Again（25 分）
date: 2017-10-16T12:14:00
tags: 树    
categories: 数据结构
---

### 题目

![](http://osxdn70ll.bkt.clouddn.com/17-10-16/25698045.jpg)



[链接](https://pintia.cn/problem-sets/900290821590183936/problems/909063377378181120)



### 分析

push是二叉树前序遍历的结果，pop是二叉树中序遍历的结果，所以这个题就是已知前序遍历和中序遍历，求后序遍历。



### AC代码



```cpp

#include "bits/stdc++.h"

using namespace std;

struct TreeNode

{

	int left=-1, right=-1;

}tree[45];

vector<int> v;

void postorder(int x) {

	if (x == -1) return;

	postorder(tree[x].left);

	postorder(tree[x].right);

	//cout << x << ' ';

	v.push_back(x);

}



int main() {

	int n, i, x, t;

	cin >> n;

	string str;

	stack<int> s;

	t = 0;

	bool p = true;

	for (i = 1; i <= 2*n; i++) {

		cin >> str;

		if (str == "Push") {

			cin >> x;

			s.push(x);

			if (p)

				tree[t].left = x;

			else

				tree[t].right = x;

			t = x;

			p = true;

		}

		else if (str == "Pop") {

			t = s.top();

			p = false;

			s.pop();



		}

		else {

			cout << "出错了！";

			return 0;

		}

	}

	//后序遍历二叉树

	postorder(0);

	

	//输出结果

	for (i = 0; i < v.size() - 1; i++) {

		cout << v[i];

		if (i < v.size() - 2)

			cout << ' ';



	}



	return 0;

}



```
    