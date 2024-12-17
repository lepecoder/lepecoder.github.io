---
title: leetCodelinked-list-cycle-ii找到链表的环
date: 2017-07-31T20:26:00
tags:
categories: 数据结构
---

## 题目


>Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:

Can you solve it without using extra space?


## 分析


给你一个链表,要求返回链表中环的开始位置,如果没有环则返回NULL

尽量不占用额外的存储空间



参考牛客网[wangxiaobao的回答](https://www.nowcoder.com/questionTerminal/6e630519bf86480296d0f1c868d425ad)



1. 使用快慢指针判断是否有环,如果有环则快慢指针必定会相遇

2. 定义两个指针分别在链表的开始位置和快慢指针的相遇位置,以相同的速度前进,两指针相遇的位置就是链表中环的开始位置.



__证明如下:__



![linked-list-cycle-ii](http://uploadfiles.nowcoder.com/images/20150812/122270_1439340467801_QQ%E6%88%AA%E5%9B%BE20150812084712.jpg)



1. X是链表开始位置,Y是环的入口,Z是快慢指针相遇位置.a b c是长度

2. 两指针在Z处相遇时,快指针走的长度是慢指针的2倍,则有以下等式成立:



```

2*(a+b) = a+b+n*(b+c)

推出:

a = n*(b+c)-b

```



根据上面的公式,我们可以让一个指针从X处开始,另一个指针从Z处开始,当第一个指针走了a的距离时,第二个指针刚好回退b的距离,退到环的开始位置和第一个指针相遇



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

    ListNode *detectCycle(ListNode *head) {

        if(!head) return head;

        ListNode * fast = head;

        ListNode * slow = head;

        //快慢指针相遇,则表示有环

        while(fast && fast->next){

            slow = slow->next;

            fast = fast->next->next;

            if(fast == slow) break;

        }

        //如果没有相遇, 返回NULL

        if(!fast || !fast->next) return NULL;

        slow = head;

        while(slow != fast){

            slow = slow->next;

            fast = fast->next;

        }

        return slow;

    }

};

```
    