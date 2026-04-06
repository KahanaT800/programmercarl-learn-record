---
title: "代码随想录打卡第34天 | 62. 不同路径、63. 不同路径II、343. 整数拆分、96. 不同的二叉搜索树"
date: 2026-04-06
tags: [动态规划]
---

## 62. 不同路径

- 链接：[62. 不同路径](https://leetcode.cn/problems/unique-paths/)
- 文章：[代码随想录](https://programmercarl.com/0062.不同路径.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ve4y1x7Eu)
- 状态：✅

### 第一想法
第一个想法就是看 dp[1][1] 的值应该是多少，然后就想到
`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`

但是接着想一条边上的值就会发现，只要在最左和最上，不管走多远，**选择**都是 1；

所以初始化的时候要初始化这两条边为 1；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int i = 0; i < n; i++) {
            dp[0][i] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

## 63. 不同路径II

- 链接：[63. 不同路径II](https://leetcode.cn/problems/unique-paths-ii/)
- 文章：[代码随想录](https://programmercarl.com/0063.不同路径II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Ld4y1k7c6)
- 状态：✅

### 第一想法
要思考有障碍物的 DP 递推公式，注意到有障碍物的 dp[i][j] 为 0；

然后是初始化的时候障碍物会截断后面的为 0；

### 看完题解后的想法
--

### 实现中遇到的困难
有很多多余代码，精简了下

### 代码

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));

        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) {
            dp[i][0] = 1;
        }

        for (int i = 0; i < n && obstacleGrid[0][i] == 0; i++) {
            dp[0][i] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

## 343. 整数拆分（一刷可跳过）

- 链接：[343. 整数拆分](https://leetcode.cn/problems/integer-break/)
- 文章：[代码随想录](https://programmercarl.com/0343.整数拆分.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Mg411q7YJ)
- 状态：❌

### 第一想法
没啥思路，dp 代表什么没想明白

### 看完题解后的想法
dp[i] 代表 i 这个数的最大拆分乘积；

然后需要遍历循环看是把这个数拆成两半的值更大，还是其中一个数还要继续拆比较大；

### 实现中遇到的困难
在状态转移的时候，注意还需要与原来的 dp[i] 比较

### 代码

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1, 0);
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j < i; j++) {
                dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
            }
        }

        return dp[n];
    }
};
```

## 96. 不同的二叉搜索树（一刷可跳过）

- 链接：[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)
- 文章：[代码随想录](https://programmercarl.com/0096.不同的二叉搜索树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1eK411o7QA)
- 状态：❌

### 第一想法
试图找规律，但是没看出来

### 看完题解后的想法
理解分开的树的序号和 dp[i] 之间的关系；

卡特兰数：以 j 为根时，左子树有 j-1 个节点、右子树有 i-j 个节点，方案数相乘再对所有根求和

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[i - j] * dp[j - 1];
            }
        }
        return dp[n];
    }
};
```

## 今日收获
