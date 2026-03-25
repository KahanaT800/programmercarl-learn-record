---
title: "代码随想录打卡第22天 | 77. 组合、216. 组合总和III、17. 电话号码的字母组合"
date: 2026-03-25
tags: [回溯]
---

## 回溯算法理论基础

- 文章：[代码随想录 - 回溯算法理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1cy4y167mM)

## 77. 组合

- 链接：[77. 组合](https://leetcode.cn/problems/combinations/)
- 文章：[代码随想录](https://programmercarl.com/0077.组合.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ti4y1L7cv)
- 剪枝：[B站讲解](https://www.bilibili.com/video/BV1wi4y157er)
- 状态：❌

### 第一想法
第一想法是回溯，但是对于如何存 path，如何回溯有点摸不着头脑;

### 看完题解后的想法
不用复杂的递归返回，只把递归作为前进的路径，直接使用外部空间保存路径;

外部空间必须在退出递归时 pop 数据，这就是回溯;

### 实现中遇到的困难
剪枝条件写起来很复杂，但是搞清楚了如何剪枝就能马上写出来;

### 代码

```cpp
class Solution {
public:
    /*
    回溯的模板可以这样理解：
    1. 路径 (path)：当前已经选了哪些数
    2. 选择列表：从 start 到 n，还能选哪些数
    3. 结束条件：path.size() == k 时，收集结果
    */
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(int n, int k, int start) {
        // 返回条件：找到k个数
        if (path.size() == k) {
            res.push_back(path);
            return;
        }

        /*
        剪枝优化：
        当前已有数字 path.size() 个, 还需数字 k - path.size() 个
        在 i 到 n 之间有 n - i + 1个数字
        n - i + 1 >= k - path.size()
        */
        for (int i = start; i <= n + 1 + path.size() - k; i++) {
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return res;
    }
};
```

## 216. 组合总和III

- 链接：[216. 组合总和III](https://leetcode.cn/problems/combination-sum-iii/)
- 文章：[代码随想录](https://programmercarl.com/0216.组合总和III.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1wg411873x)
- 状态：✅

### 第一想法
回溯三部曲：
1. 路径选择
2. 选择列表
3. 返回条件

\+ 剪枝优化

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(int k, int n, int start, int sum) {
        if (path.size() == k && sum == n) {
            res.push_back(path);
            return;
        }

        // 剪枝优化：数量优化 + 和优化
        for (int i = start; i <= 10 - (k - path.size()) && sum < n; i++) {
            path.push_back(i);
            sum += i;
            backtracking(k, n, i + 1, sum);
            path.pop_back();
            sum -= i;
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k, n, 1, 0);
        return res;
    }
};
```

## 17. 电话号码的字母组合

- 链接：[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)
- 文章：[代码随想录](https://programmercarl.com/0017.电话号码的字母组合.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1yV4y1V7Ug)
- 状态：✅

### 第一想法
这道题的核心点就在于每次是在不一样的集合中把 data 加进 path；

每层都直接遍历，只需要让 index 达到边界就能自然返回；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<string> res;
    string path;
    vector<vector<char>> maps = {{'a', 'b', 'c'}, // 2
                                 {'d', 'e', 'f'}, // 3
                                 {'g', 'h', 'i'}, // 4
                                 {'j', 'k', 'l'}, // 5
                                 {'m', 'n', 'o'}, // 6
                                 {'p', 'q', 'r', 's'}, // 7
                                 {'t', 'u', 'v'}, // 8
                                 {'w', 'x', 'y', 'z'}, // 9
                                };

    void backtracking(string digits, int index) {
        if (index == digits.size()) {
            res.push_back(path);
            return;
        }

        int number = digits[index] - '0';
        for (int i = 0; i < maps[number - 2].size(); i++) {
            path += maps[number - 2][i];
            backtracking(digits, index + 1);
            path.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        backtracking(digits, 0);
        return res;
    }
};
```

## 今日收获
