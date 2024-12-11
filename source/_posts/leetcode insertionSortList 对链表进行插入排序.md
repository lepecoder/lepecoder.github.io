---
title: leetcode insertionSortList 对链表进行插入排序
date: 2017-07-30T13:12:00
tags:
categories:
---

## __描述:__
>Sort a linked list using insertion sort.

使用插入排序对一个链表进行排序


普通的插入排序,时间复杂度O(n^2)

```cpp
class Solution {
public:
    ListNode * insertionSortList(ListNode * head) {
        ListNode dummy(0);
        ListNode * cur = head;
        ListNode * pre;
        while(cur){
            //记录链表的下一个结点,用于cur后移
            ListNode * next = cur->next;
            pre = &dummy;
            //跳过小于cur->val的结点
            while(pre->next && pre->next->val < cur->val)
                pre = pre->next;
            //将cur插入pre->next 前
            cur->next = pre->next;
            pre->next = cur;
            cur = next;
        }
        return dummy.next;
    }
};
```
    