---
title: Leetcode 905, 剑指 Offer 21
description: 给定一个非负整数数组A，返回一个数组，在该数组中，A的所有偶数元素之后跟着所有奇数元素。
tags:
  - Leetcode
  - 快排划分
date: "2020-12-31"
---

* 描述
  * 给定一个非负整数数组A，返回一个数组，在该数组中，A的所有偶数元素之后跟着所有奇数元素。你可以返回满足此条件的任何数组作为答案。
* 解法
  * 类似快排的划分过程(若已忘记则参考王道/大话的快排算法)
  * 移动oddStart和evenEnd的过程中用**循环**而不是**if**
    ```cxx
    class Solution {
        public:
            vector<int> sortArrayByParity(vector<int>& A) {
                int evenEnd = 0, oddStart = A.size() - 1;
                while (evenEnd < oddStart) {
                    while (evenEnd < oddStart && A[oddStart] % 2) {
                        --oddStart;
                    }
                    while (evenEnd < oddStart && A[evenEnd] % 2 == 0) {
                        ++evenEnd;
                    }
                    if (evenEnd < oddStart) {
                        swap(A[evenEnd], A[oddStart]);
                        ++evenEnd;
                        --oddStart;
                    }
                }
                return A;
            }
        };
    ```
## 剑指Offer-21
* 描述
  * 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
* 解法
  * 与上题相同
    ```go
    func exchange(nums []int) []int {
        oddStart, evenEnd := 0, len(nums) - 1
        for oddStart < evenEnd {
            for oddStart < evenEnd && nums[oddStart] % 2 == 1 {
                oddStart++
            }
            for oddStart < evenEnd && nums[evenEnd] % 2 == 0 {
                evenEnd--
            }
            if oddStart < evenEnd {
                nums[oddStart], nums[evenEnd] = nums[evenEnd], nums[oddStart]
            }
        }
        return nums
    }
    ```