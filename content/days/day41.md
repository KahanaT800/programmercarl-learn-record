---
title: "代码随想录打卡第41天 | 121. 买卖股票的最佳时机、122. 买卖股票的最佳时机II、123. 买卖股票的最佳时机III"
date: 2026-04-13
tags: [动态规划, 买卖股票]
---

## 121. 买卖股票的最佳时机

- 链接：[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)
- 文章：[代码随想录](https://programmercarl.com/0121.买卖股票的最佳时机.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Xe4y1u77q)
- 状态：⚠️

### 第一想法
动态规划写法不会

### 看完题解后的想法
dp[i][0] 表示第 i 天持有股票的最大利润，dp[i][1] 表示第 i 天不持有股票的最大利润

注意买卖一次和买卖多次的区别

### 实现中遇到的困难
要注意理解 dp 数组的含义一直都是记录的最大利润；

所以在算今天卖出获得收益时是今天的收益 + 昨天的收益

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if (size < 2) return 0;
        // dp[i][0] 表示第 i 天持有股票的最大利润，dp[i][1] 表示第 i 天不持有股票的最大利润
        vector<vector<int>> dp(size, vector<int>(2, 0));
        dp[0][0] = -prices[0]; // 第0天持股
        dp[0][1] = 0; // 第0天不持股
        for (int i = 1; i < size; i++) {
            // 持股：要么是前一天就持股，要么是今天持股
            // 注意这里当天持股是 -prices[i]，这种写法就是说明买入股票是一次性操作，没有考虑之前不持股的收益
            dp[i][0] = max(dp[i-1][0], -prices[i]);
            // 不持股：要么是前一天就不持股，要么是前一天持股，今天卖出获得收益
            dp[i][1] = max(dp[i-1][1], prices[i] + dp[i-1][0]);
        }

        return dp[size - 1][1];
    }
};
```

## 122. 买卖股票的最佳时机II

- 链接：[122. 买卖股票的最佳时机II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)
- 文章：[代码随想录](https://programmercarl.com/0122.买卖股票的最佳时机II（动态规划）.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1D24y1Q7Ls)
- 状态：✅

### 第一想法
买卖股票 DP 写法的拓展，重点在于持股的时候，是用前一天不持股的收益减去买股票的支出，还是用 0（前一天不持股说明没买没卖）减去

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if (size < 2) return 0;
        // dp[i][0] 表示第 i 天持有股票的最大利润，dp[i][1] 表示第 i 天不持有股票的最大利润
        vector<vector<int>> dp(size, vector<int>(2, 0));
        dp[0][0] = -prices[0]; // 第0天持股
        dp[0][1] = 0; // 第0天不持股
        for (int i = 1; i < size; i++) {
            // 持股：要么是前一天就持股，要么是今天持股
            // 注意可以多次买卖，所以要用前一天不持股的收益减去买股票的支出
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]);
            // 不持股：要么是前一天就不持股，要么是前一天持股，今天卖出获得收益
            dp[i][1] = max(dp[i-1][1], prices[i] + dp[i-1][0]);
        }

        return dp[size - 1][1];
    }
};
```

## 123. 买卖股票的最佳时机III

- 链接：[123. 买卖股票的最佳时机III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)
- 文章：[代码随想录](https://programmercarl.com/0123.买卖股票的最佳时机III.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1WG411K7AR)
- 状态：❌

### 第一想法
没想出来

### 看完题解后的想法
dp[i][j]，其中 i 表示第 i 天，j 表示状态，一共有五种状态；

### 实现中遇到的困难
要理解五种状态与状态的相互转化；

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(5, 0));
        // 5种状态
        // 0 ：不买不卖
        // 1 ：第一次持有股票
        // 2 ：第一次卖出股票
        // 3 ：第二次持有股票
        // 4 : 第二次卖出股票
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = dp[i - 1][0];
            // 第 i 天第一次持有股票要么是前一天已经持有，要么是前一天啥也不做这天持有
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            // 第 i 天第一次卖出股票要么是前一天已经卖出，要么是前一天是持有状态这天卖出
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            // 第 i 天第二次持有股票要么是前一天已经持有，要么是前一天是卖过一次的状态这天持有
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            // 第 i 天第二次卖出股票要么是前一天已经卖出，要么是前一天是第二次持有的状态这天卖出
            dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }
        return dp[n - 1][4];
    }
};
```

## 今日收获
