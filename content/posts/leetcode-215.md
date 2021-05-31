---
title: Leetcode 215
description: 未排序的数组中找到第k个最大的元素
tags:
  - Leetcode
  - 快速排序
date: "2020-12-19"
---

## 描述
* 在未排序的数组中找到第k个最大的元素
## 解法
* 快速排序思想
* 建堆取队首
## 快速排序思想
* 总体
  * O(n)时间实现扫描一遍数组, 基准左边元素都小于基准, 基准右边元素都大于基准
* 扫描方法
  * 取第一个数为基准(pivot), left为基准的下标, right为最后一个元素的下标
  * 当`left < right`且right对应元素应在基准右侧时(如从小到大排, right对应元素大于基准), dec(right)
  * 否则left与right交换, 即将这个元素和pivot交换, 交换前left指向pivot, 交换后right指向pivot
  * 当`left < right`且left对应元素应在基准左侧时(如从小到大排, left对应元素小于基准), inc(left)
  * 否则left与right交换, 即将这个元素和pivot交换, 交换前right指向pivot, 交换后left指向pivot
  * 回到第二步继续循环, 直到left>=right终止
* 另法
  * 第0个元素为基准, 第1个元素开始到第k个为应在左侧的元素, 第k个到第-1个元素为应在右侧的元素
  * 扫描数组同时维护k
```cxx
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        return findHelper(nums, 0, nums.size(), k - 1);
    }
private:
    int findHelper(vector<int>& nums, int begin, int end, int k) {
        int left = begin, right = end - 1;
        int pivot = nums[left];
        while (left < right)
        {
            /* 此时pivot在left的位置 
             * 因此判断右侧是否满足条件
             */
            while (left < right && nums[right] <= pivot)
                --right;
            /* 将不满足条件的交换至满足条件 */
            swap(nums[left], nums[right]);
            /* 此时pivot在right的位置 
             * 因此判断左侧是否满足条件
             */
            while (left < right && nums[left] >= pivot)
                ++left;
            /* 将不满足条件的交换至满足条件 */
            swap(nums[left], nums[right]);
        }
        /* left == right */
        if (left == k)
            return nums[left];
        else if (k < left)
            return findHelper(nums, begin, left, k);
        else
            return findHelper(nums, left + 1, end, k);
    }
};
```