---
title: Leetcode 85
description: 一个链表拆分
tags:
  - Leetcode
  - 链表
date: "2020-12-31"
---

* 描述
  * Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
* 思路
  * 直接模拟, 将原表分拆为两张表, 一张为小于的元素, 一张为大于等于的元素
  ```cxx
  class Solution {
    public:
        ListNode* partition(ListNode* head, int x) {
            ListNode *smallerHead = nullptr;
            ListNode *largerHead = nullptr;
            ListNode *curSmaller = nullptr;
            ListNode *curLarger = nullptr;
            ListNode *cur = head;
            while (cur)
            {
                if (cur->val < x)
                {
                    if (!smallerHead)
                        smallerHead = curSmaller = cur;
                    else
                    {
                        curSmaller->next = cur;
                        curSmaller = curSmaller->next;
                    }
                }
                else
                {
                    if (!largerHead)
                        largerHead = curLarger = cur;
                    else
                    {
                        curLarger->next = cur;
                        curLarger = curLarger->next;
                    }
                }
                cur = cur->next;
            }
            if (curLarger)
                curLarger->next = nullptr;
            if (!curSmaller)
                smallerHead = largerHead;
            else
                curSmaller->next = largerHead;
            return smallerHead; 
        }
    };
  ```
  ```python
  class Solution:
        def partition(self, head: ListNode, x: int) -> ListNode:
            if (not head):
                return None
            smallerHead, largerHead = ListNode(), ListNode()
            smallerFlag, largerFlag = True, True
            curSmaller, curLarger = smallerHead, largerHead
            cur = head
            while (cur):
                if (cur.val < x):
                    if (smallerFlag):
                        smallerHead = curSmaller = cur
                        smallerFlag = False
                    else:
                        curSmaller.next = cur
                        curSmaller = curSmaller.next
                else:
                    if (largerFlag):
                        largerHead = curLarger =  cur
                        largerFlag = False
                    else:
                        curLarger.next = cur
                        curLarger = curLarger.next
                cur = cur.next
            if (largerFlag):
                largerHead = None
            else:
                curLarger.next = None
            if (smallerFlag):
                smallerHead = largerHead
            else:
                curSmaller.next = largerHead
            return smallerHead
  ```