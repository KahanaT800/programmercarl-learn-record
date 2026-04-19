---
title: "代码随想录打卡第46天 | 647. 回文子串、516. 最长回文子序列"
date: 2026-04-18
tags: [动态规划, 子序列]
---

## 647. 回文子串

- 链接：[647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)
- 文章：[代码随想录](https://programmercarl.com/0647.回文子串.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV17G4y1y7z9)
- 状态：⚠️

### 第一想法
在 dp 的过程中统计有多少个回文串；

### 看完题解后的想法
中心拓展法可以优化空间；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        // dp[i][j] 表示从下标 i 到下标 j 的子串是不是回文
        vector<vector<int>> dp(n, vector<int>(n, false));
        int sum = 0;

        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                // 如果 s[i] != s[j]，两端不一样，肯定不是回文
                // 如果 s[i] == s[j]，两端一样，那要看里面的部分
                if (s[i] == s[j]) {
                    if (j - i <= 1) {
                        // j - i <= 1，没有字符和1个字符都是回文串
                        dp[i][j] = true;
                    } else {
                        // 否则看左右往内移动一步的是不是回文串
                        // 这里的条件由 j - i <= 1 保证不超限
                        dp[i][j] = dp[i+1][j-1];
                    }
                }
                if (dp[i][j] == true) {
                    sum++;
                }
            }
        }
        return sum;
    }
};
```

## 516. 最长回文子序列

- 链接：[516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/0516.最长回文子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1d8411K7W6)
- 状态：⚠️

### 第一想法
回文子序列的长度计算和子串的区别在于内部不用一定是回文的，所以可以直接 +2；

之后如果两端不一致，内部可能是回文序列所以要考虑去掉左或者去掉右的最大值；

### 看完题解后的想法
--

### 实现中遇到的困难
因为 dp 转移中用了 max，所以最大值会被转移到最后，不用一个单独的变量记录 max；

### 代码

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        // dp[i][j] 表示从下标 i 到下标 j 的回文子序列的长度
        vector<vector<int>> dp(n, vector<int>(n, 0));

        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                // 如果 s[i] != s[j]，两端不一样，去掉左或者去掉右有可能是回文串
                // 如果 s[i] == s[j]，两端一样，那至少两端是回文串
                if (s[i] == s[j]) {
                    if (i == j) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = dp[i+1][j-1] + 2;
                    }
                } else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][n - 1];
    }
};
```

## 今日收获
