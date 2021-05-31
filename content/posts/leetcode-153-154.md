---
title: Leetcode 153, 154, 剑指 Offer 11
description: 三个二分法的例题
tags:
  - Leetcode
  - 二分
date: "2021-02-02"
---
* 描述(153):
  * 假设按照升序排序的数组在预先未知的某个点上进行了旋转。找出其中最小的元素。（数组元素唯一）
* 描述(154):
  * 假设按照升序排序的数组在预先未知的某个点上进行了旋转。找出其中最小的元素。（数组元素**不唯一**）
* 思路:
  * 二分查找。在本题中，二分查找用于逐步筛除不在查找范围中的元素，直到仅剩下数组中的最小值
  * 在数组元素唯一的情况下，我们可以通过比较基准点(pivotpos = left + (right-left) / 2)和查找范围中的最后一个点逐步筛出
    * 如果$num[pivotpos] > num[right]$, 说明pivotpos**及其**之前的点不可能为最小值, $left = left + 1$
    * 如果$num[pivotpos] < num[right]$, 说明pivotpos之后的点不可能为最小值, $right = mid$
  * 在数组元素不唯一的情况下，若遇到$num[pivotpos] == num[right]$，我们不能给出任何推断
    * e.g. `[3,3,1,3]` left = 0, right = 3, pivotpos = 1. 在pivotpos和right之间存在最小值
    * 但我们可以推断最小值一定不在right处(如果最小值在right处, 那么pivotpos和right之间所有值均为最小, right指针同样可以前移, 否则, 最小值不在right处)
  ```go
    func findMin(nums []int) int {
        left, right := 0, len(nums) - 1
        // [left, right] a range of numbers to observe
        for left < right {
            pivotpos := left + (right - left) / 2
            if (nums[pivotpos] < nums[right]) {
                // but behind has no possibility
                // pivotpos may be the minpos...
                right = pivotpos
            } else {
                // before and left has no possibility
                left = pivotpos + 1
            }
        }
        return nums[left]
    }
  ```
  ```go
    func findMin(nums []int) int {
        left, right := 0, len(nums) - 1
        for (left < right) {
            mid := left + (right - left) / 2
            if (nums[mid] < nums[right]) {
                right = mid
            } else if (nums[mid] > nums[right]){
                left = mid + 1;
            } else {
                // nums[mid] == nums[right] 
                // 不等价于[mid, right]每个元素都相等
                // e.g. [3,3,1,3] left, right, mid = 0, 3, 1
                // 只能说明right不会在可能的最小值范围内
                right--;
            }
        }
        return nums[left];
    }
  ```