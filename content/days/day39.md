---
title: "代码随想录打卡第39天 | 198. 打家劫舍、213. 打家劫舍II、337. 打家劫舍III"
date: 2026-04-11
tags: [动态规划, 打家劫舍]
---

## 198. 打家劫舍

- 链接：[198. 打家劫舍](https://leetcode.cn/problems/house-robber/)
- 文章：[代码随想录](https://programmercarl.com/0198.打家劫舍.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Te411N7SX)
- 状态:✅

### 第一想法
打家劫舍，`位置 i` 的最大收入就是 `不偷位置 i` 和 `偷位置 i 和 i-2` 的最大收入

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if (size == 1) {
            return nums[0];
        }
        vector<int> dp(size + 1, 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < size; i++) {
            dp[i] = max(dp[i-1], nums[i] + dp[i-2]);
        }

        return dp[size - 1];
    }
};
```

## 213. 打家劫舍II

- 链接：[213. 打家劫舍II](https://leetcode.cn/problems/house-robber-ii/)
- 文章：[代码随想录](https://programmercarl.com/0213.打家劫舍II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1oM411B7xq)
- 状态：⚠️

### 第一想法
既然首尾不能同时取，就把这个切成 [0, n-1] 和 [1, n] 两段分别求最大值；

### 看完题解后的想法
--

### 实现中遇到的困难
因为第二段实际是需要输入长度大于等于 3，所以需要处理两种边界情况；

### 代码

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if (size == 1) return nums[0];
        if (size == 2) return max(nums[0], nums[1]);
        vector<int> dp1(size, 0);
        dp1[0] = nums[0];
        dp1[1] = max(nums[0], nums[1]);
        for (int i = 2; i < size - 1; i++) {
            dp1[i] = max(dp1[i - 1], nums[i] + dp1[i - 2]);
        }

        vector<int> dp2(size, 0);
        dp2[0] = nums[1];
        dp2[1] = max(nums[1], nums[2]);
        for (int i = 2; i < size - 1; i++) {
            dp2[i] = max(dp2[i - 1], nums[i+1] + dp2[i - 2]);
        }

        return max(dp1[size-2], dp2[size-2]);
    }
};
```

## 337. 打家劫舍III

- 链接：[337. 打家劫舍III](https://leetcode.cn/problems/house-robber-iii/)
- 文章：[代码随想录](https://programmercarl.com/0337.打家劫舍III.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1H24y1Q7sY)
- 状态：⚠️

### 第一想法
不知道怎么递归

### 看完题解后的想法
树形 DP，因为要看 root 偷不偷需要看左右子树是否要偷，所以要用后序遍历

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> dfs(TreeNode* root) {
        if (!root) return {0, 0}; // 不偷，偷
        // 后序遍历：需要比较左右子树的偷与不偷
        vector<int> left = dfs(root->left);
        vector<int> right = dfs(root->right);
        int notRob = max(left[0], left[1]) + max(right[0], right[1]);
        int rob = root->val + left[0] + right[0];
        return {notRob, rob};
    }

    int rob(TreeNode* root) {
        vector<int> res = dfs(root);
        return max(res[0], res[1]);
    }
};
```

## 今日收获
