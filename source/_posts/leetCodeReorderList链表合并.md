---
title: leetCodeReorderList链表合并
date: 2017-07-31T18:04:00
tags:
categories: 数据结构
---

## 原题

>Given a singly linked list L: L0?L1?…?Ln-1?Ln,

reorder it to: L0?Ln?L1?Ln-1?L2?Ln-2?…

You must do this in-place without altering the nodes' values.

For example,

Given {1,2,3,4}, reorder it to {1,4,2,3}.



## 分析

将链表结点按照第一个 倒数第一个 第二个 倒数第二个 的顺序重新排序



如 `1 2 3 4`重新排序后是 `1 4 2 3`



## AC代码



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

    //翻转链表

    ListNode *reverse(ListNode *head){

        if(!head) return;

        ListNode * pre = NULL;

        while(head){

            ListNode * nex = head->next;

            head->next = pre;

            pre = head;

            head = nex;

        }

        return pre;

    }

    void reorderList(ListNode* head) {

        if(!head || !head->next) return head;

        ListNode * fast = head;

        ListNode * slow = head;

        while(fast->next && fast->next->next){

            fast = fast->next->next;

            slow = slow->next;

        }

        fast = slow->next;

        //截断

        slow->next = NULL;

        //翻转后半段

        ListNode * p = reverse(fast);



        //合并

        ListNode * res = head;

        ListNode * q = head->next;

        while(p && q){

            res->next = p;

            p = p->next;

            res = res->next;



            res->next = q;

            q = q->next;

            res = res->next;

        }

        if(p) res->next = p;

        if(q) res->next = q;

    }

};

```
    