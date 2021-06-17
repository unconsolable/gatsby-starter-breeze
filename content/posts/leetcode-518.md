---
title: Leetcode 347
description: 零钱兑换II, 背包变种
tags:
  - Leetcode
  - 背包
date: "2021-06-17"
---

## 描述
给你一个整数数组 coins 表示不同面额的硬币, 另给一个整数 amount 表示总金额. 

请你计算并返回可以凑成总金额的硬币组合数. 如果任何硬币组合都无法凑出总金额, 返回 0 . 

假设每一种面额的硬币有无限个.  

题目数据保证结果符合 32 位带符号整数. 

## 思路
* `dp[i]`表示金额为i时的硬币组合数, 可以递推得到结果

* 但在之前的求解中经常会出现重复求解的问题

* 经过阅读题解和分析后, 发现问题在于之前的写法将amount作为外层循环, 而coins作为内层循环, 即每个阶段求解用所有硬币表示一定金额的方案. 这样求解时会有重复计算的情况

* 而题解给的思路时将外层和内层循环顺序颠倒, 这样能**确保金额之和为i的硬币面额的顺序**.

```go
func change(amount int, coins []int) int {
    sort.Ints(coins)
    dp := make([]int, amount+1)
    dp[0] = 1
    // coins first, then amount
    // to avoid count twice
    for _, v := range coins {
        for j := v; j <= amount; j++ {
            if dp[j-v] != 0 {
                dp[j] += dp[j-v];
            }
        }
    }
    return dp[amount];
}
```
