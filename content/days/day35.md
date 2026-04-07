---
title: "代码随想录打卡第35天 | 01背包问题（二维/一维）、416. 分割等和子集"
date: 2026-04-07
tags: [动态规划, 背包]
---

## KamaCoder 46. 携带研究材料（01背包）

- 链接：[KamaCoder 46. 携带研究材料(01背包)](https://kamacoder.com/problempage.php?pid=1046)
- 文章：[代码随想录 - 01背包二维](https://programmercarl.com/背包理论基础01背包-1.html)
- 视频：[B站讲解 - 二维](https://www.bilibili.com/video/BV1cg411g7Y6)
- 一维文章：[代码随想录 - 01背包一维](https://programmercarl.com/背包理论基础01背包-2.html)
- 一维视频：[B站讲解 - 一维](https://www.bilibili.com/video/BV1BU4y177kY)
- 状态：❌

### 第一想法
01背包忘干净了；

### 看完题解后的想法
首先是要明白 dp[i][j] 代表什么：i 是物品序号，j 是背包重量，存的是最大总价值；

然后是背包的初始化，重量为 0 的所有背包价值为 0，然后第一个物品的重量满足后的所有列的值是第一个物品的价值；

之后是放入第二个物品的考虑，如果当前重量小于当前物品，则**继承上一行的价值**（因为有可能上一个物品能放进来），如果能放进当前物品，则要比较是
**放入当前物品+剩下的容量放其他物品** 和 **不放入当前物品直接继承上一行的价值** 哪种更大；

### 实现中遇到的困难
--

### 代码

二维 DP：
```cpp
#include <iostream>
#include <vector>

using namespace std;

int bag(vector<int>& weight, vector<int>& value, int n) {
    int m = weight.size();
    vector<vector<int>> dp(m, vector<int>(n+1, 0));
    for (int j = weight[0]; j <= n; j++) {
        dp[0][j] = value[0];
    }

    for (int i = 1; i < m; i++) {
        for (int j = 1; j <= n; j++) {
            if (j < weight[i]) {
                dp[i][j] = dp[i-1][j];
            } else {
                dp[i][j] = max(dp[i-1][j], value[i] + dp[i-1][j - weight[i]]);
            }
        }
    }

    return dp[m-1][n];
}

int main() {
    int m, n;
    cin >> m >> n;

    vector<int> weight(m);
    vector<int> value(m);

    for (int i = 0; i < m; ++i) {
        cin >> weight[i];
    }
    for (int i = 0; i < m; ++i) {
        cin >> value[i];
    }

    cout << bag(weight, value, n) << endl;

    return 0;
}
```

## 416. 分割等和子集

- 链接：[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)
- 文章：[代码随想录](https://programmercarl.com/0416.分割等和子集.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1rt4y1N7jE)
- 状态：❌

### 第一想法
01背包的变体，但是还是不会

### 看完题解后的想法
这道题里 weight 和 cost 都变成了数字本身，但是如何放进背包需要转换下想法

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
        }
        if (sum % 2 == 1) return false;
        int target = sum / 2;
        vector<int> dp(target + 1, 0);

        for (int i = 0; i < n; i++) {
            for (int j = target; j >= nums[i]; j--) {
                dp[j] = max(dp[j], nums[i] + dp[j - nums[i]]);
            }
        }

        if (dp[target] == target) return true;
        return false;
    }
};
```

## 今日收获
