---
title: Leetcode 135
description: 一道贪心的题解
tags:
  - Leetcode
  - 贪心
date: "2020-12-24"
---
## 描述
* 需要按照以下要求，帮助老师给这些孩子分发糖果：
  * 每个孩子至少分配到 1 个糖果。
  * 相邻的孩子中，评分高的孩子必须获得更多的糖果。
## 解法
* 贪心, 两边扫描
  ```cxx
    class Solution {
    public:
        int candy(vector<int>& ratings) {
            vector<int> ret(ratings.size(), 1);
            for (int i = 1; i < ratings.size(); ++i)
            {
                if (ratings[i - 1] < ratings[i])
                    ret[i] = ret[i - 1] + 1;
            }
            for (int i = ratings.size() - 1; i > 0; --i)
            {
                if (ratings[i] < ratings[i - 1])
                    ret[i - 1] = max(ret[i] + 1, ret[i - 1]);
            }
            int sum = accumulate(ret.begin(), ret.end(), 0);
            return sum;
        }
    };
  ```