---
title: "代码随想录打卡第31天 | 56. 合并区间、738. 单调递增的数字、968. 监控二叉树"
date: 2026-04-03
tags: [贪心, 二叉树]
---

## 56. 合并区间

- 链接：[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)
- 文章：[代码随想录](https://programmercarl.com/0056.合并区间.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1wx4y157nD)
- 状态：✅

### 第一想法
区间判重，更新 end；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end()); // 核心，先排序
        int start = intervals[0][0];
        int end = intervals[0][1];
        vector<vector<int>> res;

        for (int i = 1; i < (int)intervals.size(); i++) {
            int left = intervals[i][0];
            int right = intervals[i][1];

            if (left <= end) {
                if (right > end) {
                    end = right;
                } else {
                    continue;
                }
            } else {
                res.push_back({start, end});
                start = left;
                end = right;
            }
        }
        // 最后一组元素需要手动push
        res.push_back({start, end});
        return res;
    }
};
```

## 738. 单调递增的数字

- 链接：[738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)
- 文章：[代码随想录](https://programmercarl.com/0738.单调递增的数字.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Kv4y1x7tP)
- 状态：⚠️

### 第一想法
第一想法是拿个 vector 把数字都装起来，但是之后怎么做不明白；

### 看完题解后的想法
关键是从后遍历 + 遇到不递增的位置，因为该位置需要置为 9（也不可能是 9），所以前一位就要下移 1 位；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string s = to_string(n);
        int flag = s.size();
        for (int i = flag - 1; i > 0; i--) {
            if (s[i - 1] > s[i]) {
                // 从后到前，记录第一个不递增的位置
                flag = i;
                // 前一项自减1（因为它的后一项在下一步会置为9，肯定是要下移1位）
                s[i-1]--;
            }
        }
        for (int i = flag; i < s.size(); i++) {
            // 不递增的位置开始全部设为9
            s[i] = '9';
        }
        return stoi(s);
    }
};
```

## 968. 监控二叉树（一刷可跳过）

- 链接：[968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)
- 文章：[代码随想录](https://programmercarl.com/0968.监控二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1SA411U75i)
- 状态：❌

### 第一想法
叶子节点的父节点要装摄像头，但是如何进行状态变化不会；

### 看完题解后的想法
很巧妙；

### 实现中遇到的困难
对于递归返回值这种写法的利用不够熟练，不习惯通过返回值来进行状态判断；

### 代码

```cpp
class Solution {
public:
    int res = 0;
    int traverse(TreeNode* node) {
        // 0：未被覆盖
        // 1：已放摄像头
        // 2：已被覆盖（但没放摄像头）
        if (!node) {
            return 2; // 空节点视为被覆盖
        }
        int left = traverse(node->left);
        int right = traverse(node->right);
        // 情况1
        // 左右节点都有覆盖，说明该点就没人管且没摄像头
        if (left == 2 && right == 2) {
            return 0;
        }

        // 情况2
        // 如果左右任有一个没被覆盖的，该点就需要放一个摄像头
        if (left == 0 || right == 0) {
            res++;
            return 1;
        }

        // 情况3
        // 如果左右任有一个摄像头，该点就被覆盖
        if (left == 1 || right == 1) {
            return 2;
        }
        return -1;
    }
    int minCameraCover(TreeNode* root) {
        if (traverse(root) == 0) {
            res++;  // 根节点没被覆盖，必须自己放一个
        }
        return res;
    }
};
```

## 今日收获
