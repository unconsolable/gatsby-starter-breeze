---
title: Leetcode 160
description: 找到两个单链表相交的起始节点
tags:
  - Leetcode
  - 双指针
date: "2020-12-31"
---

* 描述
  * 编写一个程序, 找到两个单链表相交的起始节点.
* 解法
  * 双指针解法, 一个指针指向headA, 一个指针指向headB
  * 最后两个指针必相交, 无论是否有相交节点
  * 因为指针循环次数至多均为A链表+B链表长度
  * 如果次数达到最大次数, 说明无相交节点, 返回None
  * 如果次数小于最大次数, 说明有相交节点, 返回相交节点
  ```python
  class Solution:
      def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
          curA, curB = headA, headB
          # curA == curB =>
          # find intersction
          # or
          # both are None
          while (curA != curB):
              curA = curA.next if curA else headB
              curB = curB.next if curB else headA
          return curA
  ```
# 类似题目
## Leetcode-141, 142 链表判环, 找入环第一个节点
* 描述
  * 给定一个链表, 判断链表中是否有环, 找入环第一个节点.
* 解法
  * 设置快慢指针, 快指针跳两个节点, 慢指针跳一个节点, 快慢指针相交, 说明快指针比慢指针多走一个链表长度, 慢指针走过路程等于环长度, 重置快指针到链表头, 快慢指针以相同速度行进, 设入环的第一个节点到相交点距离为l, 则两指针走(k-l)次后再次相遇, 相遇位置则为环起点
  ```python
  class Solution:
      def detectCycle(self, head: ListNode) -> ListNode:
          fast = slow = head
          while (fast != None):
              if (fast.next != None):
                  fast = fast.next.next
              else:
                  fast = None
                  break
              slow = slow.next
              if (fast == slow):
                  break
          if (fast == None or slow == None):
              return None
          slow = head
          while (fast != slow):
              fast = fast.next
              slow = slow.next
          return slow
  ```
## 剑指Offer 22 找倒数第k个节点
* 描述
  * 输入一个链表, 输出该链表中倒数第k个节点.(最后一个节点为倒数第1个节点)
* 解法
  * 快指针比慢指针多走k步, 快指针到链表尾后位置时, 慢指针到倒数第k个位置
  ```python
  class Solution:
      def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
          first, second = head, head
          while (second and k):
              second = second.next
              k -= 1
          if (k):
              return None
          while (second):
              first = first.next
              second = second.next
          return first
  ```