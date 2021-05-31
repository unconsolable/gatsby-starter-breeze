---
title: Leetcode 160
description: 最...的k个元素问题
tags:
  - Leetcode
  - Heap
  - STL
date: "2020-12-31"
---

## 描述
* 有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点
  * 距离为欧几里得距离
* 最大/最小的K个元素问题, 相似问题: 215(最大), 347(对次数计算)
## 解法
### 快速排序思想
* 先引入215中对这类问题的讨论
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
* 这一题同样可以使用这种方法(实现暂略, 类似215)
### 构造最X堆(和题意相符的堆)直接查询
* 比如求最大K个元素, 就构造一个最大堆, 再popK个元素
  * 使用STL中priority_queue
    ```cxx
    struct point
    {
        int index = 0;
        int dist = 0;
    };

    bool compare(const point& lhs, const point& rhs)
    {
        return lhs.dist > rhs.dist;
    }
  
    class Solution {
    public:
        vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
            // auto compare = [](const point& lhs, const point& rhs) -> bool
            //                {
            //                    return lhs.dist > rhs.dist;
            //                };
            priority_queue<point,vector<point>,decltype(&compare)> pq(&compare);
            vector<vector<int>> ret;
            for (int i = 0; i < points.size(); ++i)
            {
                point p1 = {i, points[i][0]*points[i][0] + points[i][1]*points[i][1]};
                pq.push(p1);
            }
            for (int i = 0; i < K; ++i)
            {
                auto cur = pq.top();
                pq.pop();
                ret.emplace_back(points[cur.index]);
            }
            return ret;
        }
    };
    ```
  * 自制堆
    ```cxx
    class Solution
    {
    public:
        vector<vector<int>> kClosest(vector<vector<int>> &points, int K)
        {
            auto compare = [this](const std::vector<int> &lhs, const std::vector<int> &rhs) {
                return dist(lhs[0], lhs[1]) > dist(rhs[0], rhs[1]);
            };
            auto sink = [&compare, &points](unsigned index) {
                // adjust
                while (2 * index + 1 < points.size())
                {
                    if (2 * index + 2 < points.size() && compare(points[2 * index + 1], points[2 *  index + 2]))
                    {
                        if (compare(points[index], points[2 * index + 2]))
                        {
                            swap(points[index], points[2 * index + 2]);
                            index = 2 * index + 2;
                        }
                    }
                    else if (compare(points[index], points[2 * index + 1]))
                    {
                        swap(points[index], points[2 * index + 1]);
                        index = 2 * index + 1;
                    }
                    else
                      break;
              }
          };
          // adjust
          for (int i = points.size() / 2 - 1; i >= 0; --i)
          {
              // adjust
              sink(i);
          }
          // select
          std::remove_reference_t<decltype(points)> ret;
          for (int i = 0; i < K; ++i)
          {
              ret.push_back(points[0]);
              points[0] = points[points.size() - 1];
              points.pop_back();
              unsigned index = 0;
              // adjust
              sink(index);
          }
          return ret;
      }

      private:
          inline int dist(int x, int y)
          {
              return x * x + y * y;
          }
      };
      ```
### 构造最Y堆(和题意相反的堆)间接查询
* 思路
  * 找最小K个元素时建立最大堆, 先把前K个元素加入堆中, 从K+1个元素开始判断, 若这个元素小于堆顶元素, 则堆顶元素一定不是最小元素, 则pop堆顶元素并push此元素, 最后找到最小的K个元素
* 解法(使用priority_queue)
    ```cxx
    class Solution {
    public:
        vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
            auto compare = [this](const std::vector<int> &lhs, const std::vector<int> &rhs) {
                return dist(lhs[0], lhs[1]) < dist(rhs[0], rhs[1]);
            };
            priority_queue<vector<int>, vector<vector<int>>, decltype(compare)> pq(compare); 
            for (int i = 0; i < K; ++i)
                pq.push(points[i]);
            for (int i = K; i < points.size(); ++i)
            {
                auto& cur = pq.top();
                if (dist(points[i][0], points[i][1]) < dist(cur[0], cur[1]))
                {
                    pq.pop();
                    pq.push(points[i]);
                }
            }
            vector<vector<int>> ret;
            while (!pq.empty())
            {
                ret.emplace_back(move(pq.top()));
                pq.pop();
            }
            return ret;
        }
    private:
        inline int dist(int x, int y)
        {
            return x * x + y * y;
        }
    };
    ```
### 两行解法
  ```cxx
  class Solution {
  public:
      vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
          nth_element(points.begin(), points.begin() + K, points.end(), [](vector<int>& u, vector<int>& v) {
              return u[0] * u[0] + u[1] * u[1] < v[0] * v[0] + v[1] * v[1];
          });
          return vector<vector<int>>(points.begin(), points.begin() + K);
      }
  };
  ```
  * nth_element[介绍](https://zh.cppreference.com/w/cpp/algorithm/nth_element)
    * ```cxx
      template< class RandomIt > void nth_element( RandomIt first, RandomIt nth, RandomIt last );
      ```
    * > nth_element是部分排序算法，它重排[first, last)中元素，使得：  
      > nth 所指向的元素被更改为假如 [first, last) 已排序则该位置会出现的元素。  
      > 这个新的 nth 元素前的所有元素小于或等于新的 nth 元素后的所有元素。