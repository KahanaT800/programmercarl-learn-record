---
title: "代码随想录打卡第24天 | 93. 复原IP地址、78. 子集、90. 子集II"
date: 2026-03-27
tags: [回溯]
---

## 93. 复原IP地址

- 链接：[93. 复原IP地址](https://leetcode.cn/problems/restore-ip-addresses/)
- 文章：[代码随想录](https://programmercarl.com/0093.复原IP地址.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1XP4y1U73i)
- 状态：⚠️

### 第一想法
和分割回文串类似，不是去构造一个合法的子串，而是去直接分割，不合法就 skip；

### 看完题解后的想法
--

### 实现中遇到的困难
最开始 path 是一个 str，对于 IP 地址要求的四段判断比较复杂；

path 在这里也需要是 vector，这样回溯才方便；最后直接构造一个返回字符串就行；

### 代码

```cpp
class Solution {
public:
    bool isValid(string& s) {
        if (s.size() > 1 && s[0] == '0') return false;
        if (stoi(s) > 255) return false;
        return true;
    }
    vector<string> res;
    vector<string> path;
    void backtracking(string s, int index) {
        if (path.size() == 4) {
            if (index == s.size()) {
                res.push_back(path[0] + "." + path[1] + "." + path[2] + "." + path[3]);
            }
            return;  // 不管是否收集，4段了就不再递归
        }

        for (int i = index; i < s.size() && i - index < 3; i++) {
            string str = s.substr(index, i - index + 1);
            if (isValid(str)) {
                path.push_back(str);
                backtracking(s, i+1);
                path.pop_back();
            } else {
                break; // 当前不合法后面更长也不合法
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        if (s.size() > 12 || s.size() < 4) {
            return res;
        }
        backtracking(s, 0);
        return res;
    }
};
```

## 78. 子集

- 链接：[78. 子集](https://leetcode.cn/problems/subsets/)
- 文章：[代码随想录](https://programmercarl.com/0078.子集.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1U84y1q7Ci)
- 状态：✅

### 第一想法
自己做的时候考虑到符合条件的子集大小是变动的，就在回溯外面套了一个 while 循环去增加 size；

再在回溯里判断 path 的 size 是不是满足 size 大小；

### 看完题解后的想法
其实这里直接不加任何判断条件的回溯三部曲就是子集。

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, int index) {
        res.push_back(path);

        for (int i = index; i < nums.size(); i++) {
            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return res;
    }
};
```

## 90. 子集II

- 链接：[90. 子集II](https://leetcode.cn/problems/subsets-ii/)
- 文章：[代码随想录](https://programmercarl.com/0090.子集II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1vm4y1F71J)
- 状态：✅

### 第一想法
LC40 同款同层去重

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
    void backtracking(vector<int>& nums, int index) {
        res.push_back(path);

        for (int i = index; i < nums.size(); i++) {
            if (i > index && nums[i] == nums[i - 1]) continue;
            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        backtracking(nums, 0);
        return res;
    }
};
```

## 今日收获
