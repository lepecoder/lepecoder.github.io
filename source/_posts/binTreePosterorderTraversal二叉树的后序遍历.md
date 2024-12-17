---
title: binTreePosterorderTraversal二叉树的后序遍历
date: 2017-07-30T21:21:00
tags: 二叉树
categories: 数据结构
---

__描述:__

>Given a binary tree, return the postorder traversal of its nodes' values.



```

For example:

Given binary tree {1,#,2,3},

   1

    \

     2

    /

   3

return [3,2,1].

```



直接递归,按照 左子树->右子树->头结点的顺序



AC代码:



```cpp

/**

 * Definition for a binary tree node.

 * struct TreeNode {

 *     int val;

 *     TreeNode *left;

 *     TreeNode *right;

 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}

 * };

 */

#include <vector>

class Solution {

public:

    std::vector<int> v;

    vector<int> postorderTraversal(TreeNode* root) {

        if(root){

            postorderTraversal(root->left);

            postorderTraversal(root->right);

            v.push_back(root->val);

        }

        return v;

    }

};

```
    