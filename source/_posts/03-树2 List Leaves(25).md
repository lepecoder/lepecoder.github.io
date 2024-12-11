---
title: 03-树2 List Leaves(25)
date: 2017-10-12T12:50:00
tags:
categories:
---

### 题目
![](http://osxdn70ll.bkt.clouddn.com/17-10-12/44657321.jpg)

### 分析
输入先给出结点的数量，把结点从0开始标号，每一行给出结点的左右两个子节点，`-`表示子节点不存在。

很容易分析出在子节点中没有出现的就是根节点，两个子节点都为空的是叶子节点

先建树，然后从root结点广度优先搜索，搜索到叶子节点就搜索，需要注意的是因为要求输出的顺序是从上到下、从左到右，因此如果左右子节点都不为空则应先`push`左子节点。
![](http://osxdn70ll.bkt.clouddn.com/17-10-12/31433700.jpg)
### AC代码

```cpp
#include "bits/stdc++.h"
using namespace std;

struct TreeNode
{
	int left, right;
}tree[14];

int main() {
	int n, i;
	cin >> n;
	string l, r;
	bool isRoot[14];
	memset(isRoot, 1, sizeof(isRoot));
	for (i = 0; i < n; i++) {
		cin >> l >> r;
		if (l[0] != '-') {
			tree[i].left = stoi(l);
			isRoot[tree[i].left] = 0;
		}
		else
			tree[i].left = -1;
		if (r[0] != '-') {
			tree[i].right = stoi(r);
			isRoot[tree[i].right] = 0;
		}
		else
			tree[i].right = -1;
	}
	//找到根结点
	int root;
	for (i = 0; i < n; i++) {
		if (isRoot[i]) root = i; 
	}
	//cout << "根节点: " << root << endl;
	queue<int> q;
	q.push(root);
	vector<int> v;
	while (!q.empty()) {
		int t = q.front();
		//cout << t << ' ';
		q.pop();
		if (tree[t].left == -1 && tree[t].right == -1)
			v.push_back(t);
		if (tree[t].left != -1)
			q.push(tree[t].left);
		if (tree[t].right != -1)
			q.push(tree[t].right);

	}
	//cout << endl;
	for (i = 0; i < v.size(); i++) {
		cout << v[i];
		if (i != v.size() - 1)
			cout << ' ';
	}

	

	return 0;
}

```
    