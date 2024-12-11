---
title: leetcode Sort List 对链表进行排序
date: 2017-07-30T03:00:00
tags:
categories:
---

__描述:__
>Sort a linked list in O(n log n) time using constant space complexity.

在O(n*log(n))的时间复杂度,常数级空间复杂度内对一个链表进行排序
采用归并排序,用快慢指针将链表分成两部分,最后合并两个链表.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(!head || !head->next) return head;
        ListNode * p = head;
        ListNode * q = head->next;
        //二分
        while(q && q->next){
            p = p->next;
            q = q->next->next;
        }
        ListNode * right = sortList(p->next);    //对后半段递归排序
        p->next = NULL; //把前后两段分开
        ListNode * left = sortList(head);
        //合并前后两段,实际上是用这里的判断大小来排序
        return merge(left, right);
    }

    // 合并函数
    ListNode *merge(ListNode *l, ListNode *r){
        ListNode res(0);
        ListNode * p = &res;
        //按增序合并l和r
        while(l && r){
            if(l->val < r->val){
                p->next = l;
                l = l->next;
            }else{
                p->next = r;
                r = r->next;
            }
            p = p->next;
        }
        if(l) p->next = l;
        if(r) p->next = r;
        return res.next;
    }
};
```
    