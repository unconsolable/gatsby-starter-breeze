---
title: Leetcode 3, 424 
description: 两道双指针例题
tags:
  - Leetcode
  - 双指针
date: "2021-02-02"
---

* 描述
  * 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
* 解法
  * 滑动窗口，左指针指向字串的左侧，右指针指向字串的右侧，右指针不断向右移动，若出现重复字符，则左指针右移至去重
  * 说明:本方法等价于枚举不含重复字符子串的右侧边界，找到对应的左侧边界。若右侧边界右移一次(right++)后仍不含重复字符，则左侧边界保持，否则左侧边界右移直到条件满足。
  ```go
  func lengthOfLongestSubstring(s string) int {
      char2cnt := make(map[byte]int)
      left, right, length := 0, 0, 0
      for right < len(s) {
          value, present := char2cnt[s[right]]
          if (!present || value == 0) {
              char2cnt[s[right]] = 1
              right++
          } else {
              char2cnt[s[right]] += 1
              if (right - left > length) {
                  length = right - left
              }
              for (left < right && char2cnt[s[right]] > 1) {
                  char2cnt[s[left]]--;
                  left++;
              }
              right++;
          }
      }
      if (right - left > length) {
          length = right - left
      }
      return length
  }
  ```
# Leetcode-424
* 描述
  * 给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。
* 思路
  * 与上题类似，枚举字符串的右侧边界，找到对应的左侧边界。左侧边界满足左右侧边界之间的字符除去占次数最大的字符外，其他字符所占次数小于k的情况时，不再移动；不满足时，只向左移动一次。
  * 这样移动之后的子串可能不满足条件，但整个字符串中存在满足条件的，这个长度的子串
  ```go
  func max(x int, y int) int {
      if (x > y) {
          return x
      }
      return y
  }
  
  func characterReplacement(s string, k int) int {
      var cnt [26]int
      left, right, maxcnt := 0, 0, 0
      for right < len(s) {
          cnt[s[right] - 'A']++
          maxcnt = max(maxcnt, cnt[s[right] - 'A'])
          if (right - left - maxcnt + 1 > k) {
              cnt[s[left] - 'A']--
              left++
          }
          right++
      }
      return right - left
  }
  ```