---
title: Leetcode 104
description: 二叉树最大深度
tags:
  - Leetcode
  - 递归
  - 层序遍历
date: "2021-02-02"
---

## 描述
给定一个二叉树，找出其最大深度
## 解法
* 递归法
* 非递归法
  * 层序遍历
  * 一个counter记录当前遍历的节点个数, 一个curCnt记录当前层的最后一个节点的个数, 一个nextCnt记录下一层最后一个节点的个数
  * $counter == curCnt$时状态转移: $curCnt = nextCnt, ++depth$
  * 求解某层节点个数, 每层节点个数, 树的宽度等同样适用
    ```cxx
    class Solution {
        public:
            int maxDepth(TreeNode* root) {
                queue<TreeNode *> q;
                if (root)
                    q.push(root);
                int depth = 0, curCnt = 1, nextCnt = 1, counter = 0;
                while (!q.empty())
                {
                    auto curNode = q.front();
                    q.pop();
                    ++counter;
                    if (curNode->left)
                    {
                        ++nextCnt;
                        q.push(curNode->left);
                    }
                    if (curNode->right)
                    {
                        ++nextCnt;
                        q.push(curNode->right);
                    }
                    if (counter == curCnt)
                    {
                        ++depth;
                        curCnt = nextCnt;
                    }
                }
                return depth;  
            }
        };
    ```
  