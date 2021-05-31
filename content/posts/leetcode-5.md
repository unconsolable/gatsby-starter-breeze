---
title: Leetcode 5
description: 找出最长回文子串 题解
tags:
  - Leetcode
  - 最长子串
  - 区间DP
date: "2020-12-31"
---

* 描述
  找出最长回文子串
* 解法
  * 区间dp(n2)
    * 设`dp[i][j]`表示`[i,j)`是否为回文串, 则
      * $dp[i][i]=true$ 空串作为回文串
      * $dp[i][i+1]=true$ 长度为1的串作为回文串
      * 当`dp[i+1][j]`为真并且`dp[i] = dp[j]`时, `dp[i][j+1]`为真
      ```python
      class Solution:
          def longestPalindrome(self, s: str) -> str:
              # dp[i][j] [i,j)
              dp = [[ False for i in range(len(s) + 1)] for i in range(len(s))]
              for i in range(len(s)):
                  dp[i][i] = True
                  dp[i][i + 1] = True
              start, end = 0, 1
              for i in range(2, len(s) + 1):
                  for j in range(len(s) - i + 1):
                      # [j, i + j)
                      # [j + 1, i + j) => [j, i + j + 1)
                      if (dp[j + 1][i + j - 1] and s[j] == s[i + j - 1]):
                          dp[j][i + j] = True
                          if (i > end - start):
                              start = j
                              end = i + j
              return s[start:end]
      ```
  * 暴力枚举(n3)
  * Hash+二分求回文半径(nlogn)
  * 马拉车算法(n)