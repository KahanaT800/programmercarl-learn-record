---
title: "代码随想录打卡第37天 | 完全背包理论、518. 零钱兑换II、377. 组合总和IV、70. 爬楼梯进阶版"
date: 2026-04-09
tags: [动态规划, 背包]
---

## KamaCoder 52. 携带研究材料（完全背包）

- 链接：[KamaCoder 52. 携带研究材料(完全背包)](https://kamacoder.com/problempage.php?pid=1052)
- 文章：[代码随想录](https://programmercarl.com/背包问题理论基础完全背包.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1uK411o7c9)
- 状态：❌

### 第一想法
--

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>

using namespace std;

int Bag(const vector<int>& weight, const vector<int>& value, int cap) {
    int size = weight.size();
    vector<int> dp(cap + 1, 0);

    for (int i = 1; i <= size; i++) {
        for (int j = weight[i - 1]; j <= cap; j++) {
            dp[j] = max(dp[j], dp[j - weight[i - 1]] + value[i - 1]);
        }
    }

    return dp[cap];
}

int main() {
    int m, n;
    cin >> m >> n;

    vector<int> weight(m);
    vector<int> value(m);

    for (int i = 0; i < m; ++i) {
        cin >> weight[i] >> value[i];;
    }

    cout << Bag(weight, value, n) << endl;

    return 0;
}
```

## 518. 零钱兑换II

- 链接：[518. 零钱兑换II](https://leetcode.cn/problems/coin-change-ii/)
- 文章：[代码随想录](https://programmercarl.com/0518.零钱兑换II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1KM411k75j)
- 状态：⚠️

### 第一想法
完全背包的求组合问题

### 看完题解后的想法
注意 dp[0] = 1 的初始化，代表什么都不选也是方案；

然后就是完全背包是从左扫到右，01背包是从右扫到左；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // dp[j]：凑成总金额j的货币组合数为dp[j]
        vector<uint64_t> dp(amount+1, 0); // 防止相加数据超int
        dp[0] = 1; // 什么都不选也是一种方案

        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] += dp[j - coins[i]]; // dp[j - coins[i]]，对于j这个数，我在这个位置能给一个coins[i]，那剩下的部分由之前的方案给出
            }
        }

        return dp[amount];
    }
};
```

## 377. 组合总和IV

- 链接：[377. 组合总和IV](https://leetcode.cn/problems/combination-sum-iv/)
- 文章：[代码随想录](https://programmercarl.com/0377.组合总和Ⅳ.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1V14y1n7B6)
- 状态：❌

### 第一想法
如何把不同顺序记录下来没想出来；

### 看完题解后的想法
组合数：先遍历物品，再遍历容量 - 每个物品按顺序放入，自然排除了顺序颠倒的情况

排列数：先遍历容量，再遍历物品 - 对于一个容量来说，所有的物品都有可能能放进去

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<uint64_t> dp(target+1, 0);
        dp[0] = 1;

        // 排列数，先遍历容量，再遍历物品：
        for (int j = 1; j <= target; j++) {
            for (int i = 0; i < nums.size(); i++) {
                if (j >= nums[i]) {
                    dp[j] += dp[j - nums[i]];
                }
            }
        }

        return dp[target];
    }
};
```

## KamaCoder 57. 爬楼梯（进阶版）

- 链接：[KamaCoder 57. 爬楼梯(进阶版)](https://kamacoder.com/problempage.php?pid=1067)
- 文章：[代码随想录](https://programmercarl.com/0070.爬楼梯完全背包版本.html)
- 状态：⚠️

### 第一想法
完全背包的排列数问题

组合数：先遍历物品，再遍历容量

排列数：先遍历容量，再遍历物品

在这里楼梯阶数就是容量，步长就是物品；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>

using namespace std;
int climbStairs(int n, int m) {
    vector<int> dp(n+1, 0);
    dp[0] = 1;

    for (int j = 1; j <= n; j++) {
        for (int i = 1; i <= m; i++) {
            if (j >= i) {
                dp[j] += dp[j - i];
            }
        }
    }
    return dp[n];
}

int main() {
    int n, m;
    cin >> n >> m;
    cout << climbStairs(n, m) << endl;
    return 0;
}
```

## 今日收获
