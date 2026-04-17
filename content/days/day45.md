---
title: "代码随想录打卡第45天 | 115. 不同的子序列、583. 两个字符串的删除操作、72. 编辑距离"
date: 2026-04-17
tags: [动态规划, 子序列, 编辑距离]
---

## 115. 不同的子序列

- 链接：[115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)
- 文章：[代码随想录](https://programmercarl.com/0115.不同的子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1fG4y1m75Q)
- 状态：❌

### 第一想法
LCS 架构但是转移方程写不出来；

### 看完题解后的想法
dp 数组的含义不太一样，理解了 dp 的含义转移方程的含义才好理解；

### 实现中遇到的困难
谁加的 unsigned long long;

### 代码

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.size();
        int n = t.size();
        // dp[i][j] 代表 s 的前 i 个字符中，t 的前 j 个字符作为子序列出现的次数
        vector<vector<unsigned long long>> dp(m + 1, vector<unsigned long long>(n + 1, 0));
        for (int i = 0; i <= m; i++) {
            // t为空串时任何 s 都有一种方式匹配
            dp[i][0] = 1;
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s[i - 1] == t[j - 1]) {
                    // 当 s[i-1] == t[j-1] 时，两种选择
                    // 用 s[i-1] 和 t[j-1] 配对：方案数是 dp[i-1][j-1]
                    // 不用 s[i-1]，跳过它：方案数是 dp[i-1][j]
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    // 不相等时考虑s减去一个字符考虑
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[m][n];
    }
};
```

## 583. 两个字符串的删除操作

- 链接：[583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/)
- 文章：[代码随想录](https://programmercarl.com/0583.两个字符串的删除操作.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1we4y157wB)
- 状态：⚠️

### 第一想法
和 LC72 很像，但是简单一点；

### 看完题解后的想法
考虑的时候忘掉了 word1 如果是空的那 word2 要删掉自己 j 次，其实和 LC72 的如果 word1 是空的那么 word1 要插入自己 j 次一个道理；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        // dp[i][j]：把word1的前i个字符变得和word2的前j个字符一样需要的步骤
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        // 对于空的word2，word1需要删除i次
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        // 对于空的word1，word2需要删除i次
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    // 相等的不动
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // 不相等的时候word1 或 word2 就删掉这个字符
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
                }
            }
        }

        return dp[m][n];
    }
};
```

## 72. 编辑距离

- 链接：[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)
- 文章：[代码随想录](https://programmercarl.com/0072.编辑距离.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1qv4y1q78f)
- 状态：❌

### 第一想法
大脑一片空白；

### 看完题解后的想法
这道题的 dp 数组和转移方程都很难理解；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        // dp[i][j] 表示 word1 前 i 个字符变成 word2 前 j 个字符所需的最小步数
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 0; i <= m; i++) {
            // 把word1的前i个字符变成空字符需要删除i次
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            // 把空字符变成word2的前j个字符需要插入j次
            dp[0][j] = j;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    // 如果 word1[i-1] == word2[j-1]，不需要操作
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // 删除 word1[i-1]：dp[i-1][j] + 1
                    // 把 word1 最后一个字符扔掉，问题变成 word1 前 i-1 个字符变成 word2 前 j 个字符，即 dp[i-1][j]

                    // 在 word1 插入 word2[j-1]：dp[i][j-1] + 1
                    // 在 word1 末尾插入 word2[j-1]，这样 word1 末尾和 word2 末尾就匹配了，问题变成 word1 前 i 个字符变成 word2 前 j-1 个字符，即 dp[i][j-1]

                    // 替换 word1[i-1] 为 word2[j-1]：dp[i-1][j-1] + 1
                    // 把 word1 最后一个字符改成和 word2 最后一个字符一样，两个末尾匹配了，问题变成 word1 前 i-1 个字符变成 word2 前 j-1 个字符，即 dp[i-1][j-1]
                    dp[i][j] = min({dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1});
                }
            }
        }
        return dp[m][n];
    }
};
```

## 今日收获
