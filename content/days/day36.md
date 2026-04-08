---
title: "代码随想录打卡第36天 | 1049. 最后一块石头的重量II、494. 目标和、474. 一和零"
date: 2026-04-08
tags: [动态规划, 背包]
---

## 1049. 最后一块石头的重量II

- 链接：[1049. 最后一块石头的重量II](https://leetcode.cn/problems/last-stone-weight-ii/)
- 文章：[代码随想录](https://programmercarl.com/1049.最后一块石头的重量II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV14M411C7oV)
- 状态：⚠️

### 第一想法
找出尽可能相等的两堆，然后互相减；类似于 LC416，但是有点说不清楚含义；

### 看完题解后的想法
dp[j] 的含义是：容量为 j 时，从石头里选若干个，最多能凑出多少重量。

遍历完所有石头后，dp[target] 就是不超过 sum/2 的前提下能凑出的最大值，这就是较小那堆的最优解。

确实还是背包的那一套 容量 - weight - cost；在容量内找到一个尽可能 weight 最大的值；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        for (int s : stones) {
            sum += s;
        }
        int target = sum / 2;
        vector<int> dp(target+1, 0);

        for (int i = 0; i < stones.size(); i++) {
            for (int j = target; j >= stones[i]; j--) {
                dp[j] = max(dp[j], stones[i] + dp[j - stones[i]]);
            }
        }
        return sum - 2 * dp[target];
    }
};
```

## 494. 目标和

- 链接：[494. 目标和](https://leetcode.cn/problems/target-sum/)
- 文章：[代码随想录](https://programmercarl.com/0494.目标和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1o8411j73x)
- 状态：❌

### 第一想法
没想出来；

### 看完题解后的想法
只感觉真神奇，更像是一种积累的过程；

比如 [1, 1, 1, 1, 1]，凑够 1 的方案，然后在这个方案上叠加找到凑够 2 的方案，最后是凑够 3 的方案

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int s : nums) {
            sum += s;
        }
        if ((sum + target) % 2 != 0 || sum + target < 0) return 0;
        int need = (sum + target) / 2;
        vector<int> dp(need+1, 0);
        dp[0] = 1;  // 凑出0有1种方式：什么都不选

        for (int i = 0; i < nums.size(); i++) {
            for (int j = need; j >= nums[i]; j--) {
                // dp[j] 表示凑出和为 j 的方案数
                // dp[j - nums[i]]就自带了一个判断，要凑够j这个数，现在有一个num[i]，还需要的值是不是有了
                dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        return dp[need];
    }
};
```

## 474. 一和零

- 链接：[474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)
- 文章：[代码随想录](https://programmercarl.com/0474.一和零.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1rW4y1x7ZQ)
- 状态：⚠️

### 第一想法
可以把字符串数组转化为一个 cost 矩阵，就是要满足 {m, n} 这个背包最多可以放的元素个数，01背包的变体；

### 看完题解后的想法
--

### 实现中遇到的困难
怎么把 dp 扩展到二维是个技术活；

### 代码

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> cost = getNumber(strs);

        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        for (int i = 0; i < cost.size(); i++) {
            for (int j = m; j >= cost[i][0]; j--) {
                for (int z = n; z >= cost[i][1]; z--) {
                    dp[j][z] = max(dp[j][z], 1 + dp[j - cost[i][0]][z - cost[i][1]]);
                }
            }
        }
        return dp[m][n];
    }
    vector<vector<int>> getNumber(vector<string>& strs) {
        vector<vector<int>> res(strs.size(), vector<int>(2, 0));
        for (int i = 0; i < strs.size(); i++) {
            int m = 0, n = 0;
            for (char s : strs[i]) {
                if (s == '0') {
                    m++;
                } else {
                    n++;
                }
            }
            res[i][0] = m;
            res[i][1] = n;
        }
        return res;
    }
};
```

## 今日收获
