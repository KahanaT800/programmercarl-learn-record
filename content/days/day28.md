---
title: "代码随想录打卡第28天 | 122. 买卖股票的最佳时机II、55. 跳跃游戏、45. 跳跃游戏II、1005. K次取反后最大化的数组和"
date: 2026-03-31
tags: [贪心]
---

## 122. 买卖股票的最佳时机II

- 链接：[122. 买卖股票的最佳时机II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)
- 文章：[代码随想录](https://programmercarl.com/0122.买卖股票的最佳时机II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ev4y1C7na)
- 状态：✅

### 第一想法
连续上涨的利润等于每天差价之和

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        for (int i = 1; i < prices.size(); i++) {
            // 只要后一天的股价高于前一天的股价就可以买进卖出，这样一定获利
            // 多次交易只是把一段长的收入拆分成了几段小的
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i-1];
            }
        }
        return profit;
    }
};
```

## 55. 跳跃游戏

- 链接：[55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)
- 文章：[代码随想录](https://programmercarl.com/0055.跳跃游戏.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1VG4y1X7kB)
- 状态：❌

### 第一想法
最开始在思考如何到达右边，然后写了一串复杂的判断；

### 看完题解后的想法
要点不是能不能过去，而是中间会不会断

那么如果某个点的坐标大于了之前所有点的坐标+跨度，就说明这个点到达不了，就断了

所以要维护一个 `最大可达坐标`

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.size(); i++) {
            // 如果当前最大可达坐标小于当前index，说明过不来，错误
            if (i > maxReach) {
                return false;
            }
            // 更新当前最大可达坐标，这里的作用就是在这个点之后的点的跨度如果小于该点，也在该点的包括范围内
            maxReach = max(maxReach, nums[i] + i);
            if (maxReach >= nums.size() - 1) {
                return true;
            }
        }

        return false;
    }
};
```

## 45. 跳跃游戏II

- 链接：[45. 跳跃游戏II](https://leetcode.cn/problems/jump-game-ii/)
- 文章：[代码随想录](https://programmercarl.com/0045.跳跃游戏II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Y24y1r7XZ)
- 状态：⚠️

### 第一想法
可跳区域的左右是否夹了一个更大的可以跳出这个区域的点，如果有就说明可以去这个点，跳到更远的地方；

只有1个元素的情况需要特判；

### 看完题解后的想法
进一步想想，不必在意起跳点是否要更新；

只需要维护一个最远可达坐标，在遍历的过程中只有达到最远可达坐标，才++；

这种写法其实就是从手动找到最远可达坐标然后更新抽象而来，既然最后能到最远可达坐标，那就让最远可达坐标在远方等着，直到遍历找到它，然后更新；

延迟贪心；

### 实现中遇到的困难
循环的判断条件要写 `i < nums.size() - 1`，防止只有一个元素的系列进去跑一次；

### 代码
自己想的：
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int left = 0;
        int right = nums[0];
        int res = 1;

        while (right < nums.size() - 1) {
            // 记录当前右边最大可达距离，区间内是否有更远的，如果有就更新
            int nextRight = right;
            for (int i = left + 1; i <= right; i++) {
                if (nums[i] + i > nextRight) {
                    nextRight = nums[i] + i;
                    left = i;
                }
            }
            // 左边移动了，右边还没移动
            right = nextRight;
            res++;
        }

        return res;
    }
};
```
题解做法，延迟贪心：
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int res = 0;
        int maxReach = 0;
        int curEnd = 0; // 当前还没跳，所以这里是0

        for (int i = 0; i < nums.size() - 1; i++) {
            maxReach = max(maxReach, nums[i] + i);
            // 只有达到上一次找到的最远可达坐标才更新步数；
            if (i == curEnd) {
                res++;
                curEnd = maxReach;
                if (curEnd >= nums.size() - 1) {
                    break;
                }
            }
        }

        return res;
    }
};
```

## 1005. K次取反后最大化的数组和

- 链接：[1005. K次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)
- 文章：[代码随想录](https://programmercarl.com/1005.K次取反后最大化的数组和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV138411G7LY)
- 状态：✅

### 第一想法
排序k次，每次都翻转最小值；

### 看完题解后的想法
只需要排序一次，如果还需要翻转非负数，就看看翻转后的最小值是偶数次还是奇数次；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        // 先把负数翻正
        for (int i = 0; i < nums.size() && k > 0 && nums[i] < 0; i++) {
            nums[i] = -nums[i];
            k--;
        }

        int sum = 0;
        int minValue = INT_MAX;
        // k 还有剩余且是奇数，减去最小值的两倍
        for (int i = 0; i < nums.size(); i++) {
            minValue = min(minValue, nums[i]);
            sum += nums[i];
        }
        if (k % 2 == 1) {
            sum -= 2 * minValue;
        }
        return sum;
    }
};
```

## 今日收获
