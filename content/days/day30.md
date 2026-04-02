---
title: "代码随想录打卡第30天 | 452. 用最少数量的箭引爆气球、435. 无重叠区间、763. 划分字母区间"
date: 2026-04-02
tags: [贪心]
---

## 452. 用最少数量的箭引爆气球

- 链接：[452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)
- 文章：[代码随想录](https://programmercarl.com/0452.用最少数量的箭引爆气球.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1SA41167xe)
- 状态：✅

### 第一想法
不断地调整起点和终点范围，如果任意一边（其实是起点，排序之后有序）在范围内，就可以被引爆；

注意如果终点也在范围内就要调整终点位置；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end());
        int sum = 1;
        int start = points[0][0];
        int end = points[0][1];
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] <= end) {
                start = points[i][0];
                if (points[i][1] <= end) {
                    end = points[i][1];
                }
            } else {
                start = points[i][0];
                end = points[i][1];
                sum++;
            }
        }

        return sum;
    }
};
```

## 435. 无重叠区间

- 链接：[435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)
- 文章：[代码随想录](https://programmercarl.com/0435.无重叠区间.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1A14y1c7E1)
- 状态：✅

### 第一想法
跟 LC452 挺像的；

### 看完题解后的想法
注意如果有重叠要修改 end 为更小的那个，尽可能保证不会发生重叠；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        int sum = 0;
        int start = intervals[0][0];
        int end = intervals[0][1];
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < end) {
                sum++;
                end = min(end, intervals[i][1]);  // 保留结束更早的那个
            } else {
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }

        return sum;
    }
};
```

## 763. 划分字母区间

- 链接：[763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)
- 文章：[代码随想录](https://programmercarl.com/0763.划分字母区间.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV18G4y1K7d5)
- 状态：❌

### 第一想法
第一想法是维护哈希表，然后遍历的时候维护一个表，每次加一个元素都去查当前字符串内的元素是否能和哈希表匹配；

### 看完题解后的想法
太巧妙了，直接用字符的最后一个位置作为哈希表的值，这样只要维护一个字符串内的最大距离，到达这个距离说明内部所有字符都是封闭的，可以切割

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int arr[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            // 字符的最远位置在哪
            arr[s[i] - 'a'] = i;
        }

        vector<int> res;
        int start = 0;
        int end = 0;
        for (int i = 0; i < s.size(); i++) {
            // 维护一个当前字符串的最远end
            end = max(end, arr[s[i] - 'a']);
            if (i == end) {
                // 当达到最远end的时候说明内部的所有字符都在这个区间内
                res.push_back(end - start + 1);
                start = end + 1;
            }
        }

        return res;
    }
};
```

## 今日收获
