---
title: "代码随想录打卡第29天 | 134. 加油站、135. 分发糖果、860. 柠檬水找零、406. 根据身高重建队列"
date: 2026-04-01
tags: [贪心]
---

## 134. 加油站

- 链接：[134. 加油站](https://leetcode.cn/problems/gas-station/)
- 文章：[代码随想录](https://programmercarl.com/0134.加油站.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1jA411r7WX)
- 状态：❌

### 第一想法
没想出来；

### 看完题解后的想法
很巧妙，其实是思维盲区；

既然总油量大于等于总消耗有解，那么只要找到开始盈余的部分，这样剩下的油总能够支持消耗；

这里很巧妙，因为如果总量只要是大于等于，那不管前面亏了多少，后面一定剩了多少，只要不断舍去亏损的位置就行了；

和 LC53 是一个思想；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int totalDiff = 0;
        int curDiff = 0;
        int start = 0;
        // 1. 如果总油量小于总消耗，不可能跑完
        // 2. 既然总油量大于等于总消耗有解，那么就要找到开始盈余的部分，这样才能支撑跑完全程
        for (int i = 0; i < gas.size(); i++) {
            totalDiff += gas[i] - cost[i];
            curDiff += gas[i] - cost[i];
            if (curDiff < 0) {
                start = i + 1;
                curDiff = 0;
            }
        }

        return totalDiff >= 0 ? start : -1;
    }
};
```

## 135. 分发糖果

- 链接：[135. 分发糖果](https://leetcode.cn/problems/candy/)
- 文章：[代码随想录](https://programmercarl.com/0135.分发糖果.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ev4y1r7wN)
- 状态：❌

### 第一想法
没啥思路；

### 看完题解后的想法
一个位置需要满足左右两边的约束，所以需要两个方向分别扫一次；

第二次扫描的时候使用 max，保证不会让第二次的结果比第一次的小；

先定义两个"单方向最优解"：

L[i]：只考虑左约束时，i 至少需要多少糖果。从左到右扫，相邻递增就 +1，否则重置为 1。

R[i]：只考虑右约束时，i 至少需要多少糖果。从右到左扫，同理。

L[i] 已经是只看左方向的下界——不可能比这更少还满足左约束。R[i] 同理是右方向的下界。

最终答案 candy[i] = max(L[i], R[i])。

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int size = ratings.size();
        vector<int> candy(size, 1);

        for (int i = 1; i < size; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candy[i] = candy[i - 1] + 1;
            }
        }
        for (int i = size - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candy[i] = max(candy[i], candy[i + 1] + 1);
            }
        }
        int sum = 0;
        for (int s : candy) {
            sum += s;
        }
        return sum;
    }
};
```

## 860. 柠檬水找零

- 链接：[860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)
- 文章：[代码随想录](https://programmercarl.com/0860.柠檬水找零.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV12x4y1j7DD)
- 状态：✅

### 第一想法
给20找零先考虑一个10一个5，再考虑3个5，都没有就说明找不开；但是这是什么贪心；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0;
        int ten = 0;
        for (int b : bills) {
            if (b == 5) {
                five++;
            }
            if (b == 10) {
                if (five < 1) {
                    return false;
                } else {
                    five--;
                    ten++;
                }
            }
            if (b == 20) {
                // 至少需要一张5元，而且如果10元少于1张就需要3张5元
                if (five < 1 || (five < 3 && ten < 1)) {
                    return false;
                } else {
                    // 至少有一张10元就先用10元
                    if (ten >= 1) {
                        ten--;
                        five--;
                    } else {
                        five = five - 3;
                    }
                }
            }
        }

        return true;
    }
};
```

## 406. 根据身高重建队列

- 链接：[406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)
- 文章：[代码随想录](https://programmercarl.com/0406.根据身高重建队列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1EA411675Y)
- 状态：❌

### 第一想法
没有思路；

### 看完题解后的想法
贪心算法的题更像是脑筋急转弯；

比起这道题的思路，排序的 lambda 函数写法需要掌握；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        // 排序：按身高降序排，身高相同时按 k 升序排
        // 逐个插入：遍历排序后的数组，把每个人插入到结果数组的下标 k 位置
        // 为什么 k 就是插入位置？
        // 因为当前结果数组里全是身高 >= 当前这个人的人，插到位置 k 就意味着前面恰好有 k 个人，而这些人身高都 >= 自己
        sort(people.begin(), people.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] > b[0] || (a[0] == b[0] && a[1] < b[1]);
        });

        vector<vector<int>> res;
        for (auto& p : people) {
            res.insert(res.begin() + p[1], p);
        }

        return res;
    }
};
```

## 今日收获
