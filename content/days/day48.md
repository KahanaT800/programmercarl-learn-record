---
title: "代码随想录打卡第48天 | 739. 每日温度、496. 下一个更大元素I、503. 下一个更大元素II"
date: 2026-04-20
tags: [单调栈]
---

## 739. 每日温度

- 链接：[739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)
- 文章：[代码随想录](https://programmercarl.com/0739.每日温度.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1my4y1Z7jj)
- 状态：✅

### 第一想法
维护一个元素递增的单调栈存放数组下标，这样比较 top 元素和新元素，如果新元素大于 top 则说明至少 top 指向的位置找到了更高的，需要记录并弹出；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> ans(n, 0);
        stack<int> st;
        for (int i = 0; i < n; i++) {
            while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
                int top = st.top();
                st.pop();
                ans[top] = i - top;
            }
            st.push(i);
        }

        return ans;
    }
};
```

## 496. 下一个更大元素I

- 链接：[496. 下一个更大元素I](https://leetcode.cn/problems/next-greater-element-i/)
- 文章：[代码随想录](https://programmercarl.com/0496.下一个更大元素I.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1jA411m7dX)
- 状态：⚠️

### 第一想法
暴力解能解，但是如何降低时间复杂度卡住了；

主要是想不到如何一次就将 nums1 中的元素和 nums2 中的位置对应起来；

### 看完题解后的想法
很巧妙地用哈希表记录元素与下一个更大元素的对应关系

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> map;
        stack<int> st;
        for (int i = 0; i < nums2.size(); i++) {
            // 因为要找的是元素在nums2中的下一个更大元素
            // 所以用单调栈存数据
            while (!st.empty() && nums2[i] > st.top()) {
                // 用哈希表记录元素与下一个更大元素的对应关系
                map[st.top()] = nums2[i];
                st.pop();
            }
            st.push(nums2[i]);
        }

        vector<int> res(nums1.size(), -1);
        for (int i = 0; i < nums1.size(); i++) {
            if (map.count(nums1[i]) != 0) {
                res[i] = map[nums1[i]];
            }
        }

        return res;
    }
};
```

## 503. 下一个更大元素II

- 链接：[503. 下一个更大元素II](https://leetcode.cn/problems/next-greater-element-ii/)
- 文章：[代码随想录](https://programmercarl.com/0503.下一个更大元素II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV15y4y1o7Dw)
- 状态：❌

### 第一想法
没有理解如何跑两圈

### 看完题解后的想法
2 倍长度，使用 % 进行重定位

### 实现中遇到的困难
注意压的是序号

### 代码

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        stack<int> st;
        vector<int> res(n, -1);
        // 跑两圈
        for (int i = 0; i < 2 * n; i++) {
            while (!st.empty() && nums[st.top()] < nums[i % n]) {
                res[st.top()] = nums[i % n];
                st.pop();
            }
            // 只有第一圈才记录
            if (i < n) st.push(i);
        }

        return res;
    }
};
```

## 今日收获
