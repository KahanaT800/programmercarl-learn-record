---
title: "代码随想录打卡第49天 | 42. 接雨水、84. 柱状图中最大的矩形"
date: 2026-04-21
tags: [单调栈]
---

## 42. 接雨水

- 链接：[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)
- 文章：[代码随想录](https://programmercarl.com/0042.接雨水.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1uD4y1u75P)
- 状态：⚠️

### 第一想法
和 LC84 类似，但是是一层一层地算水量；

### 看完题解后的想法
--

### 实现中遇到的困难
注意没墙的情况

### 代码

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int sum = 0;
        stack<int> st; // 维护一个递减栈

        for (int i = 0; i < height.size(); i++) {
            // 栈递减，遇到比栈顶高的元素说明栈顶可以形成凹槽
            while (!st.empty() && height[st.top()] < height[i]) {
                int bottom = height[st.top()];
                st.pop();
                // 左边没有墙，不能储水
                if (st.empty()) break;
                int h = min(height[st.top()], height[i]) - bottom;
                int w = i - st.top() - 1;
                // 一层一层地算水，和LC84类似
                sum += h * w;
            }
            st.push(i);
        }

        return sum;
    }
};
```

## 84. 柱状图中最大的矩形

- 链接：[84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)
- 文章：[代码随想录](https://programmercarl.com/0084.柱状图中最大的矩形.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Ns4y1o7uB)
- 状态：⚠️

### 第一想法
利用单调栈，当出现元素小于栈顶元素时开始计算面积；

因为栈是单调递增的，所以可以知道直接取栈里面的元素作为高，至少有 1 格长度能满足条件；

栈顶是最高的所以栈内元素在计算面积时能直接用当前元素作为高乘以长度；

### 看完题解后的想法
--

### 实现中遇到的困难
边界处理的时候要注意因为右边界无法访问，所以需要提前取值；

而因为可能从当前位置一路把栈弹空，所以在处理宽度时需要考虑左边界的情况；

### 代码

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> st; // 递增栈
        int size = heights.size();
        int res = 0;

        for (int i = 0; i <= size; i++) {
            // 为了处理边界情况需要提前取值
            int cur = (i == size) ? 0 : heights[i];
            while (!st.empty() && heights[st.top()] > cur) {
                int h = heights[st.top()];
                // 需要取高之后直接弹出，因为有可能这是最后一个元素
                st.pop();
                // 如果栈空了说明弹到了末尾，需要处理边界
                int w = st.empty() ? i : i - st.top() - 1;

                int s = h * w;
                res = max(res, s);
            }
            st.push(i);
        }

        return res;
    }
};
```

## 今日收获
