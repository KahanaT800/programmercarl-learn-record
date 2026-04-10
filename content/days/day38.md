---
title: "代码随想录打卡第38天 | 322. 零钱兑换、279. 完全平方数、139. 单词拆分、多重背包"
date: 2026-04-10
tags: [动态规划, 背包]
---

## 322. 零钱兑换

- 链接：[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)
- 文章：[代码随想录](https://programmercarl.com/0322.零钱兑换.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV14K411R7yv)
- 状态：❌

### 第一想法
最少组合数不会

### 看完题解后的想法
初始化就很讲究了，然后是组合数计算

如果求组合数就是外层 for 循环遍历物品，内层 for 遍历背包。
如果求排列数就是外层 for 遍历背包，内层 for 循环遍历物品。

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1, INT_MAX);
        dp[0] = 0;

        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                if (dp[j - coins[i]] != INT_MAX) {
                    // 防止 INT_MAX + 1 溢出
                    dp[j] = min(dp[j], 1 + dp[j - coins[i]]);
                }
            }
        }

        return dp[amount] == INT_MAX ? -1 : dp[amount];
    }
};
```

## 279. 完全平方数

- 链接：[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)
- 文章：[代码随想录](https://programmercarl.com/0279.完全平方数.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV12P411T7Br)
- 状态：⚠️

### 第一想法
不是找完全平方数，而是找一堆数的平方和能不能等于这个数；

注意是完全背包，因为可以复用；最少组合数和 LC322 一样；

dp[0] 的含义跟着题目类型走：方案数题 dp[0] = 1（空集是一种方案），最少个数题 dp[0] = 0（不需要任何元素）。

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0; // 凑出 0 需要 0 个数

        for (int i = 0; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                if (dp[j - i * i] != INT_MAX) {
                    dp[j] = min(dp[j], 1 + dp[j - i * i]);
                }
            }
        }
        return dp[n];
    }
};
```

## 139. 单词拆分

- 链接：[139. 单词拆分](https://leetcode.cn/problems/word-break/)
- 文章：[代码随想录](https://programmercarl.com/0139.单词拆分.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1pd4y147Rh)
- 状态：⚠️

### 第一想法
完全背包的一种，长度是容量，可选单词是物品；

排列数：先遍历容量，再遍历物品 - 对于一个容量来说，所有的物品都有可能能放进去

### 看完题解后的想法
要判断当前给的字符长度扣出来是不是能匹配，剩下的长度有没有匹配；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int len = s.size();
        vector<bool> dp(len+1, false);
        dp[0] = true;

        for (int j = 1; j <= len; j++) {
            for (int i = 0; i < wordDict.size(); i++) {
                int wLen = wordDict[i].size();
                if (wLen <= j && dp[j - wLen] && s.substr(j - wLen, wLen) == wordDict[i]) {
                    dp[j] = true;
                }
            }
        }

        return dp[len];
    }
};
```

## KamaCoder 56. 携带矿石资源（多重背包）

- 链接：[KamaCoder 56. 携带矿石资源(多重背包)](https://kamacoder.com/problempage.php?pid=1066)
- 文章：[代码随想录](https://programmercarl.com/背包问题理论基础多重背包.html)
- 状态：⚠️

### 第一想法
没有什么想法

### 看完题解后的想法
01背包的变体，在循环中把物品完全展开成单独的一件；

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int cap, n;
    cin >> cap >> n;
    vector<int> weight(n, 0);
    vector<int> value(n, 0);
    vector<int> nums(n, 0);
    for (int i = 0; i < n; i++) cin >> weight[i];
    for (int i = 0; i < n; i++) cin >> value[i];
    for (int i = 0; i < n; i++) cin >> nums[i];

    vector<int> dp(cap+1, 0);

    for (int i = 0; i < n; i++) {
        for (int j = cap; j >= weight[i]; j--) {
            // 01背包的扩展变体，先遍历物品，再遍历容量，保证物品有序且只用一次
            for (int k = 1; k <= nums[i]; k++) {
                // 多重背包每个物品有数量限制，所以需要遍历去试探能放多少个物品
                if (j - k * weight[i] >= 0) {
                    dp[j] = max(dp[j], k * value[i] + dp[j - k * weight[i]]);
                }
            }
        }
    }

    cout << dp[cap] << endl;
}
```

## 今日收获
