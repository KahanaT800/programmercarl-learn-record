---
title: "代码随想录打卡第25天 | 491. 递增子序列、46. 全排列、47. 全排列II、51. N皇后、37. 解数独"
date: 2026-03-28
tags: [回溯]
---

## 491. 递增子序列

- 链接：[491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)
- 文章：[代码随想录](https://programmercarl.com/0491.递增子序列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1EG4y1h78v)
- 状态：❌

### 第一想法
这题不能排序，同层去重又不会了；

### 看完题解后的想法
使用 set 记录本层使用过的数据，这是更加一般的同层去重；

### 实现中遇到的困难
不会非排序的同层去重；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, int index) {
        if (path.size() >= 2) {
            res.push_back(path);
        }

        unordered_set<int> used;  // 每层一个，记录本层用过的值
        for (int i = index; i < nums.size(); i++) {
            if ((!path.empty() && nums[i] < path.back()) || used.count(nums[i])) {
                continue;
            }
            used.insert(nums[i]); // 注意退栈不用 erase，因为还在当前层
            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        backtracking(nums, 0);
        return res;
    }
};
```

## 46. 全排列

- 链接：[46. 全排列](https://leetcode.cn/problems/permutations/)
- 文章：[代码随想录](https://programmercarl.com/0046.全排列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV19v4y1S79W)
- 状态：✅

### 第一想法
排列不需要传入 index，因为每次都是从头访问；

简单的做法是加入一个额外的全局空间记录是否出现；

### 看完题解后的想法
还有 0 额外空间的交换法，之后再研究

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (used[i]) continue;
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            used[i] = false;
            path.pop_back();
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return res;
    }
};
```

## 47. 全排列II

- 链接：[47. 全排列II](https://leetcode.cn/problems/permutations-ii/)
- 文章：[代码随想录](https://programmercarl.com/0047.全排列II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1R84y1i7Tm)
- 状态：⚠️

### 第一想法
全局去重 + 当前层去重，最开始在当前层使用 set 进行去重；

### 看完题解后的想法
排列的同层去重有点绕，两种条件都能实现，但效率不同：

**去重条件一（同层去重，推荐）：**

```cpp
nums[i] == nums[i-1] && !used[i-1]
```

- `nums[i] == nums[i-1]`：当前值和前一个相同
- `!used[i-1]`：前一个值没在用（已被回撤），说明前一个是同层之前的循环中选过又撤销的 → 同层重复，跳过
- `used[i-1] == true`：前一个值正在用（是上层选的），当前层再选是合法的 → 不跳过

在第一层 `i=1` 时直接整棵子树被剪掉，效率高。

**去重条件二（同枝去重）：**

```cpp
nums[i] == nums[i-1] && used[i-1]
```

- `used[i-1] == true`：前一个正在用（上层选的），跳过 → 禁止相同值按下标顺序连续使用
- 结果正确但展开了更多无效分支，效率低

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, vector<bool>& globalSet) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (globalSet[i]) continue; // 全局去重
            if (i > 0 && nums[i] == nums[i-1] && !globalSet[i-1]) continue; // 当前层去重
            globalSet[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, globalSet);
            globalSet[i] = false;
            path.pop_back();
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<bool> globalSet(nums.size(), false);
        backtracking(nums, globalSet);
        return res;
    }
};
```

## 51. N皇后（一刷适当跳过）

- 链接：[51. N皇后](https://leetcode.cn/problems/n-queens/)
- 文章：[代码随想录](https://programmercarl.com/0051.N皇后.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Rd4y1c7Bq)
- 状态：⚠️

### 第一想法
其实是标准的回溯三部曲，但是条件判断复杂；

回溯的做法是直接放，然后检查该位置是不是合法位置，所以核心就是合法性检查函数的书写；

要检查同一行、同一列、四个斜角是不是合法；

### 看完题解后的想法
从上到下放皇后，所以只用检查同一列的上方、左上、右上；

同一行因为每次合法都去下一行，保证了只放一次，其实不需要检查；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool isValid(vector<vector<int>>& map, int rowIndex, int colIndex) {
        int size = map.size();
        // 1. 同一列上不能有
        for (int i = 0; i < size; i++) {
            if (i != rowIndex && map[i][colIndex] == 1) {
                return false;
            }
        }
        // 2. 同一行上不能有
        for (int j = 0; j < size; j++) {
            if (j != colIndex && map[rowIndex][j] == 1) {
                return false;
            }
        }
        // 3. 四个斜向不能有
        for (int i = 1; i < size; i++) {
            if (rowIndex + i < size && colIndex + i < size && map[rowIndex + i][colIndex + i] == 1) return false;
            if (rowIndex - i >= 0 && colIndex - i >= 0 && map[rowIndex - i][colIndex - i] == 1) return false;
            if (rowIndex + i < size && colIndex - i >= 0 && map[rowIndex + i][colIndex - i] == 1) return false;
            if (rowIndex - i >= 0 && colIndex + i < size && map[rowIndex - i][colIndex + i] == 1) return false;
        }
        return true;
    }
    vector<vector<string>> res;
    void backtracking(vector<vector<int>>& map, int row) {
        if (row == map.size()) {
            vector<string> path;
            string s;
            for (int i = 0; i < map.size(); i++) {
                for (int j = 0; j < map.size(); j++) {
                    s += (map[i][j] == 0) ? "." : "Q";
                }
                path.push_back(s);
                s.clear();
            }
            res.push_back(path);
            return;
        }

        for (int j = 0; j < map.size(); j++) {
            if (isValid(map, row, j)) {
                map[row][j] = 1;
                backtracking(map, row+1);
                map[row][j] = 0;
            }
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<int>> map(n, vector<int>(n, 0));
        backtracking(map, 0);
        return res;
    }
};
```

## 37. 解数独（一刷适当跳过）

- 链接：[37. 解数独](https://leetcode.cn/problems/sudoku-solver/)
- 文章：[代码随想录](https://programmercarl.com/0037.解数独.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1TW4y1471V)
- 状态：⚠️

### 第一想法
这道题的判断条件更符合直觉，比较好写；

但是这题跟 N 皇后不一样的地方是，每层至多要跑九次，所以需要双循环遍历每一个位置；

返回写的地方也和一般的回溯三部曲不太一致；

### 看完题解后的想法
二维递归是个好名词，形象地区分了在二维数组情况下，是放完一个就跑，还是每一层都要完全遍历；

### 实现中遇到的困难
最开始沿用 N 皇后的每层放一个就跑；

### 代码

```cpp
class Solution {
public:
    string number = "123456789";
    bool isValid(vector<vector<char>>& board, char input, int rowIndex, int colIndex) {
        // 1. 同一列上不能有重复数字
        for (int i = 0; i < 9; i++) {
            if (i != rowIndex && board[i][colIndex] == input) return false;
        }
        // 2. 同一行上不能有重复数字
        for (int j = 0; j < 9; j++) {
            if (j != colIndex && board[rowIndex][j] == input) return false;
        }
        // 3. 3*3块里不能有重复数字
        int row = rowIndex / 3 * 3; // 定位到每块的左上角
        int col = colIndex / 3 * 3;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (row + i == rowIndex && col + j == colIndex) continue;
                if (board[row + i][col + j] == input) return false;
            }
        }
        return true;
    }

    bool backtracking(vector<vector<char>>& board) {
        // 每层最多要跑九次，不能像 N 皇后每层找一个就跑
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') continue; // 有数字就略过
                for (char input : number) {
                    if (isValid(board, input, i, j)) {
                        board[i][j] = input;
                        if (backtracking(board)) return true; // 一路递归找到了就返回
                        board[i][j] = '.';
                    }
                }
                return false; // 数字用完都没成功，说明失败了
            }
        }
        return true; // 所有格子都填完，成功
    }
    void solveSudoku(vector<vector<char>>& board) {
        (void)backtracking(board);
    }
};
```

## 今日收获
