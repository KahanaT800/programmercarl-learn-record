---
title: "代码随想录打卡第44天 | 1143. 最长公共子序列、1035. 不相交的线、53. 最大子序和、392. 判断子序列"
date: 2026-04-16
tags: [动态规划, 子序列]
---

## 1143. 最长公共子序列

- 链接：[1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/1143.最长公共子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ye4y1L7CQ)
- 状态：⚠️

### 第一想法
类似于 LC718，但是这里因为是子序列不要求连续，所以不匹配的时候需要继承之前的最大值；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        if (m == 0 || n == 0) {
            return 0;
        }
        // dp[i][j] 表示 text1 前 i 个字符和 text2 前 j 个字符的最长公共子序列长度
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    // dp[i-1][j-1] + 1 要求新配对的两个字符都在之前匹配范围之外
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
```

## 1035. 不相交的线

- 链接：[1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/)
- 文章：[代码随想录](https://programmercarl.com/1035.不相交的线.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1h84y1x7MP)
- 状态：✅

### 第一想法
连线就是公共子序列，也就是 LCS；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
```

## 53. 最大子序和（DP 版）

- 链接：[53. 最大子序和](https://leetcode.cn/problems/maximum-subarray/)
- 文章：[代码随想录](https://programmercarl.com/0053.最大子序和（动态规划）.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV19V4y1F7b5)
- 状态：✅

### 第一想法
最大子数组和要求连续，所以如果前缀和 + 当前值 < 当前值，那直接使用当前值替代前缀和；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre = 0;
        int res = INT_MIN;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] + pre > nums[i]) {
                pre += nums[i];
            } else {
                pre = nums[i];
            }
            res = max(pre, res);
        }

        return res;
    }
};
```

## 392. 判断子序列

- 链接：[392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/0392.判断子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1tv4y1B7ym)
- 状态：✅

### 第一想法
致敬传奇 DP 题目 LCS

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int m = s.size();
        int n = t.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n] == m;
    }
};
```

## 今日收获
