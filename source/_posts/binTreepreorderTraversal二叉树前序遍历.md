---
title: binTreepreorderTraversal二叉树前序遍历
date: 2017-07-30T22:15:00
tags:
categories:
---

## 原题
>Given a binary tree, return the preorder traversal of its nodes' values.

```
For example:
Given binary tree {1,#,2,3},
   1
    \
     2
    /
   3
return [1,2,3].
```

## 分析
对二叉树进行先序遍历,即根节点->左子树->右子树

## 代码:

#### 递归:

递归代码十分简单,建立一个vector作为返回的结果,现将根节点push进去,然后递归处理左子树和右子树.

```cpp
class Solution{
public:
    std::vector<int> v;
    vector<int> preorderTraversal(TreeNode *root){
        if(root){
            v.push_back(root->val);
            preorderTraversal(root->left);
            preorderTraversal(root->right);
        }
        return v;
    }
};
```

#### 栈

可以建立一个栈,每次把根节点push进栈,然后root=root->left;直到`root`为空时,再从栈里获取top()->right,然后重复上面的动作

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if(root == NULL) return res;
        stack<TreeNode*> st;
        TreeNode* node = root;

        while(node || !st.empty()){
            if(node){
                //遍历根节点和左子树并入栈
                res.push_back(node->val);
                st.push(node);
                node = node->left;
            }//直到结点为空,换成上一个结点的右子树,再按照上面的顺序遍历
            else{
                node = st.top()->right;
                st.pop();
            }
        }
        return res;
    }
};
```
    