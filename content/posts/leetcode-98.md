---
title: Leetcode 98
description: 验证一棵树是二叉搜索树
tags:
  - Leetcode
  - 分治
  - 迭代中序
date: "2021-02-02"
---

## 描述
  * 验证一棵树是二叉搜索树
## 解法
### 元素满足上下界验证
  * 每个元素满足由父节点和祖先节点设置的上界和下界
  * INF设为(`1U<<31-1`)时对于值为INF的右节点为无法判断
  ```cxx
  class Solution {
    public:
        bool isValidBST(TreeNode* root) {
            return helper(root->left, -INF, root->val) && helper(root->right, root->val, INF);
        }
    private:
        bool helper(TreeNode* root, int64_t lower, int64_t upper)
        {
            if (!root)
                return true;
            return helper(root->left, lower, root->val) && lower < root->val && root->val < upper && helper(root->right, root->val, upper);
        }
        constexpr static int64_t INF = (1ULL << 63) - 1;
  };
  ```
### 元素中序遍历序列单增
  * 验证左子树都比根节点小, 右子树都比根节点大, 左右子树也是二叉搜索树
    ```cxx
    class Solution {
        public:
        bool isValidBST(TreeNode* root) {
            if (!root)
                return true;
            vector<int> vec;
            stack<TreeNode *> stk;
            TreeNode *cur = root;
            while(cur || !stk.empty())
            {
                if (cur)
                {
                    stk.push(cur);
                    cur = cur->left;
                }
                else
                {
                    cur = stk.top();
                    vec.emplace_back(cur->val);
                    stk.pop();
                    cur = cur->right;
                }
            }
            for (int i = 0; i < vec.size() - 1; ++i)
            {
                if (vec[i] >= vec[i + 1])
                    return false;
            }
            return true;
        }
    };
    ```