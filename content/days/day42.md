---
title: "代码随想录打卡第42天 | 188. 买卖股票的最佳时机IV、309. 最佳买卖股票时机含冷冻期、714. 买卖股票的最佳时机含手续费"
date: 2026-04-14
tags: [动态规划]
---

## 188. 买卖股票的最佳时机IV

- 链接：[188. 买卖股票的最佳时机IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)
- 文章：[代码随想录](https://programmercarl.com/0188.买卖股票的最佳时机IV.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV16M411U7XJ)
- 状态：✅

### 第一想法
多次买卖的母题，这一天卖不卖取决于前一个状态的情况；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>( 2 * k + 1, 0));
        // 2 * k + 1种状态
        // 0 ：不买不卖
        // 2 * k + 1 ：第K次持有股票
        // 2 * k ：第K次卖出股票
        for (int j = 1; j < 2 * k + 1; j++) {
            if (j % 2 == 1) {
                dp[0][j] = -prices[0];
            }
        }
        for (int i = 1; i < n; i++) {
            dp[i][0] = dp[i - 1][0];
            for (int j = 1; j < 2 * k + 1; j++) {
                if (j % 2 == 1) {
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1] - prices[i]);
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1] + prices[i]);
                }
            }
        }
        return dp[n - 1][2 * k];
    }
};
```

## 309. 最佳买卖股票时机含冷冻期

- 链接：[309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
- 文章：[代码随想录](https://programmercarl.com/0309.最佳买卖股票时机含冷冻期.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1rP4y1D7ku)
- 状态：⚠️

### 第一想法
dp[i][j]，其中 i 是天数，j 是状态，要搞清楚状态的变化

### 看完题解后的想法
--

### 实现中遇到的困难
注意最后返回要考虑所有不持有股票的情况；

最开始写了 4 种状态转移把卖出状态和冷静期状态分开了，其实冷静期一定就是昨天卖了，所以冷静期就是昨天卖了状态；

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(3, 0));
        // 3种状态
        // 0 ：持有股票
        // 1 ：不持有股票（可以买入）
        // 2 ：冷静期（不可买入）
        dp[0][0] = -prices[0];
        for (int i = 1; i < n; i++) {
            // 持有：昨天就持有，或昨天可以买入今天买了
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            // 昨天就是这个状态，或昨天是冷静期今天结束了
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][2]);
            // 冷静期：昨天持有今天卖了
            dp[i][2] = dp[i - 1][0] + prices[i];
        }
        // 需要返回所有不持有股票的状态
        return max(dp[n - 1][1], dp[n - 1][2]);
    }
};
```

## 714. 买卖股票的最佳时机含手续费

- 链接：[714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
- 文章：[代码随想录](https://programmercarl.com/0714.买卖股票的最佳时机含手续费（动态规划）.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1z44y1Z7UR)
- 状态：⚠️

### 第一想法
就是买卖股票1加了个手续费；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        // 0 ：持有股票
        // 1 ：不持有股票
        dp[0][0] = -prices[0];

        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }

        return dp[n - 1][1];
    }
};
```

## 今日收获
