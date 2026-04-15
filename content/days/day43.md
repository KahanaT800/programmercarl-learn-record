---
title: "代码随想录打卡第43天 | 300. 最长递增子序列、674. 最长连续递增序列、718. 最长重复子数组"
date: 2026-04-15
tags: [动态规划, 子序列]
---

## 300. 最长递增子序列

- 链接：[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/0300.最长上升子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ng411J7xP)
- 状态：❌

### 第一想法
没想出来

### 看完题解后的想法
重点是搞清楚 dp[i] 的含义，表达位置 i 最多可以组成多大的递增子序列；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int size = nums.size();
        if (size == 1) return 1;
        int result = 0;
        vector<int> dp(size, 1);

        for (int i = 1; i < size; i++) {
            for (int j = 0; j < i; j++) {
                // 对于每个 i，遍历它前面所有的 j，如果 nums[j] < nums[i]
                // 说明 nums[i] 可以接在 nums[j] 后面，dp[i] = max(dp[i], dp[j] + 1)
                if (nums[j] < nums[i]) {
                    // max是为了防止后面的修改之前的最大值
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

## 674. 最长连续递增序列

- 链接：[674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/0674.最长连续递增序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1bD4y1778v)
- 状态：✅

### 第一想法
因为要求连续，所以如果中间断了的话就要重置值；

感觉这道题的思想和 LC53 最大子数组和很接近，都是遇到不满足条件的地方用 prev 截断；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n = nums.size();
        int prev = 1;
        int res = 1;

        for (int i = 1; i < n; i++) {
            if (nums[i - 1] < nums[i]) {
                prev += 1;
            } else {
                prev = 1;
            }
            res = max(res, prev);
        }

        return res;
    }
};
```

## 718. 最长重复子数组

- 链接：[718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)
- 文章：[代码随想录](https://programmercarl.com/0718.最长重复子数组.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV178411H7hV)
- 状态：❌

### 第一想法
暴力解，但是不会 DP

### 看完题解后的想法
这里 dp 数组代表的含义非常的巧妙，表示以 nums1[i-1] 和 nums2[j-1] 结尾的最长公共子数组长度，这个做法在后面的回文串和最长公共子序列等等都在使用；

这个二维 DP 的定义方式是一个通用模板。两个序列的比较问题基本都是这个结构：dp[i][j] 围绕两个序列的第 i 和第 j 个元素展开，区别只在转移方程上

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        // dp[i][j] 表示以 nums1[i-1] 和 nums2[j-1] 结尾的最长公共子数组长度
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        int res = 0;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = 0;
                }
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }
};
```

## 今日收获
