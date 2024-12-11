---
title: leetCode-linkedListCycle判断链表是否有环
date: 2017-07-31T21:03:00
tags:
categories:
---

## 题目

>Given a linked list, determine if it has a cycle in it.
Follow up:
Can you solve it without using extra space?

## 分析

判断链表是否有环,采用快慢指针,如果相遇则表示有环

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
    bool hasCycle(ListNode *head) {
        if(!head || !head->next){
            return false;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while(fast->next && fast->next->next && fast != slow){
            fast = fast->next->next;
            slow = slow->next;
        }
        return fast == slow;
    }
};
```
    