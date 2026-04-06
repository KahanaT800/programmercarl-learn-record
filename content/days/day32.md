---
title: "代码随想录打卡第32天 | 509. 斐波那契数、70. 爬楼梯、746. 使用最小花费爬楼梯"
date: 2026-04-04
tags: [动态规划]
---

## 509. 斐波那契数

- 链接：[509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)
- 文章：[代码随想录](https://programmercarl.com/0509.斐波那契数.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1f5411K7mo)
- 状态：✅

### 第一想法
第一时间没写 DP，先用递归唤醒下大脑；

然后从递归的思想倒推 DP

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int fib(int n) {
        if (n == 0 || n == 1) {
            return n;
        }
        vector<int> dp(n+1, 0);
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1]+dp[i - 2];
        }
        return dp[n];
    }
};
```

## 70. 爬楼梯

- 链接：[70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)
- 文章：[代码随想录](https://programmercarl.com/0070.爬楼梯.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV17h411h7UH)
- 状态：✅

### 第一想法
DP，需要思考递推公式

### 看完题解后的想法
--

### 实现中遇到的困难
忘了 dp 初始化

### 代码

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1 || n == 2) {
            return n;
        }
        vector<int> dp(n+1, 0);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

## 746. 使用最小花费爬楼梯

- 链接：[746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)
- 文章：[代码随想录](https://programmercarl.com/0746.使用最小花费爬楼梯.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV16G411c7yZ)
- 状态：⚠️

### 第一想法
对于如何找到最后一步有点晕，dp 的定义没有搞懂

### 看完题解后的想法
dp[i] 的定义：到达第 i 台阶所花费的最少体力为 dp[i]。

可以有两个途径得到 dp[i]，一个是 dp[i-1] 一个是 dp[i-2]。

dp[i - 1] 跳到 dp[i] 需要花费 dp[i - 1] + cost[i - 1]。

dp[i - 2] 跳到 dp[i] 需要花费 dp[i - 2] + cost[i - 2]。

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int size = cost.size();
        vector<int> dp(size + 1, 0);
        for (int i = 2; i <= size; i++) {
            // dp[0]和dp[1]都不消耗
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[size];
    }
};
```

## 今日收获
