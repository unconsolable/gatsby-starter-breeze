---
title: Leetcode 347
description: 出现频率前 k 高的元素
tags:
  - Leetcode
  - STL
date: "2020-12-19"
---

## 描述
给定一个非空的整数数组，返回其中出现频率前 k 高的元素
## 解法
* 使用map记录元素对应出现次数
* 使用priority_queue记录频率前k高元素的出现次数
* 注意top为**在传入的比较函数下优先级最高的元素**, e.g. `std::greater<T>`会造出小根堆
* 自造比较函数为避免书写类型, 使用decltype推导类型
```cpp
#include <vector>
#include <map>
#include <queue>

class Solution
{
public:
    std::vector<int> topKFrequent(std::vector<int> &nums, int k)
    {
        std::map<int, int> val2cnt;
        for (const auto &i : nums)
        {
            if (val2cnt.find(i) == val2cnt.end())
                val2cnt[i] = 1;
            else
                ++val2cnt[i];
        }
        auto compareFun = [](const std::pair<int, int> &lhs, const std::pair<int, int> &rhs) { return lhs.second > rhs.second; };
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(compareFun)> heap(compareFun);
        for (auto &i : val2cnt)
        {
            if (heap.size() != k)
                heap.push(i);
            else
            {
                auto cur = heap.top();
                if (cur.second < i.second)
                {
                    heap.pop();
                    heap.push(i);
                }
            }
        }
        std::vector<int> ans;
        while (!heap.empty())
        {
            auto cur = heap.top();
            ans.push_back(cur.first);
            heap.pop();
        }
        return ans;
    }
};
```
