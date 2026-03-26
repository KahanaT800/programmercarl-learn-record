---
title: "代码随想录打卡第23天 | 39. 组合总和、40. 组合总和II、131. 分割回文串"
date: 2026-03-26
tags: [回溯]
---

## 39. 组合总和

- 链接：[39. 组合总和](https://leetcode.cn/problems/combination-sum/)
- 文章：[代码随想录](https://programmercarl.com/0039.组合总和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1KT4y1M7HJ)
- 状态：✅

### 第一想法
回溯三部曲；

### 看完题解后的想法
可以用排序加速剪枝；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int index, int sum) {
        if (sum == target) {
            res.push_back(path);
            return;
        }

        for (int i = index; i < candidates.size() && sum + candidates[i] <= target; i++) {
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i, sum);
            sum -= candidates[i];
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);
        return res;
    }
};
```

## 40. 组合总和II

- 链接：[40. 组合总和II](https://leetcode.cn/problems/combination-sum-ii/)
- 文章：[代码随想录](https://programmercarl.com/0040.组合总和II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV12V4y1V73A)
- 状态：⚠️

### 第一想法
依旧回溯三部曲，但是注意同层去重逻辑；

### 看完题解后的想法
--

### 实现中遇到的困难
最开始把去重逻辑写在了 for 循环的外面，会把同枝的合法情况也跳过了；

同层去重保证可以用

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int index, int sum) {
        if (sum == target) {
            res.push_back(path);
            return;
        }

        for (int i = index; i < candidates.size() && sum + candidates[i] <= target; i++) {
            // 同层去重
            if (i > index && candidates[i] == candidates[i - 1]) continue;
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i + 1, sum);
            sum -= candidates[i];
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);
        return res;
    }
};
```

## 131. 分割回文串

- 链接：[131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)
- 文章：[代码随想录](https://programmercarl.com/0131.分割回文串.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1c54y1e7k6)
- 状态：⚠️

### 第一想法
核心思想：不是去构造一个合法的子串，而是去分割原串，不合法就 skip；

第一想法就是暴力遍历每一种可能的子串，但是对于如何构造子串有点晕；

### 看完题解后的想法
直接把前进的 i 当作是要切的子串的下标，然后直接切子串去判断，一步一步试出当前位置的回文串；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool isPalindrome(string& s, int left, int right) {
        while (left < right) {
            if (s[left++] != s[right--]) {
                return false;
            }
        }
        return true;
    }

    vector<vector<string>> res;
    vector<string> path;
    void backtracking(string s, int index) {
        if (index == s.size()) {
            res.push_back(path);
            return;
        }

        // 暴力遍历每一个可能子串
        for (int i = index; i < s.size(); i++) {
            // 核心思想：把i当作是切子串的下标
            if (isPalindrome(s, index, i)) {
                path.push_back(s.substr(index, i - index + 1));
                backtracking(s, i + 1);
                path.pop_back();
            }
        }
    }

    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return res;
    }
};
```

## 今日收获
