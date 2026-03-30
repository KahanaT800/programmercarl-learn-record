---
title: "代码随想录打卡第27天 | 455. 分发饼干、376. 摆动序列、53. 最大子序和"
date: 2026-03-30
tags: [贪心]
---

## 贪心算法理论基础

贪心算法没有固定套路，本质就是选择每一阶段的局部最优，从而达到全局最优。

- 文章：[代码随想录 - 贪心算法理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 455. 分发饼干

- 链接：[455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)
- 文章：[代码随想录](https://programmercarl.com/0455.分发饼干.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1MM411b7cq)
- 状态：✅

### 第一想法
始终用最小可满足的条件去满足匹配；

整体复杂度由排序主导；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        // 排序后贪心
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = 0;
        int res = 0;

        for (int i = 0; i < g.size(); i++) {
            while (index < s.size() && s[index] < g[i]) {
                index++; // 无法满足，skip
            }
            if (index < s.size()) {
                // 满足条件且index合法，分配
                res++;
                index++; // 分配之后也要前进
            }
        }

        return res;
    }
};
```

## 376. 摆动序列

- 链接：[376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)
- 文章：[代码随想录](https://programmercarl.com/0376.摆动序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV17M411b7NS)
- 状态：✅

### 第一想法
遍历序列，找到每一个拐点；但是很难说清楚为什么这就是最长的；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() == 1) {
            return 1;
        }

        int preDiff = 0;
        int diff = 0;
        int res = 1;
        for (int i = 1; i < nums.size(); i++) {
            diff = nums[i] - nums[i - 1];
            if ((diff > 0 && preDiff <= 0) || (diff < 0 && preDiff >= 0)) {
                /// 仅在符合条件的情况下更新结果计数和preDiff
                res++;
                preDiff = diff;
            }
        }

        return res;
    }
};
```

## 53. 最大子序和

- 链接：[53. 最大子序和](https://leetcode.cn/problems/maximum-subarray/)
- 文章：[代码随想录](https://programmercarl.com/0053.最大子序和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1aY4y1Z7ya)
- 状态：✅

### 第一想法
这道题的核心就在于如果当前值+过去值小于当前值，说明过去值可以舍去，直接用当前值作为新的起点；

分治法不会，之后研究吧；

### 看完题解后的想法
刷了三遍，只能说记住了；

### 实现中遇到的困难
在 `{-1}` 上踩坑了

### 代码

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0];
        int sum = nums[0];

        for (int i = 1; i < nums.size(); i++) {
            sum = max(nums[i], nums[i] + sum);
            res = max(res, sum);
        }
        return res;
    }
};
```

## 今日收获
