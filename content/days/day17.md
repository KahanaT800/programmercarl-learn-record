---
title: "代码随想录打卡第17天 | 654. 最大二叉树、617. 合并二叉树、700. 二叉搜索树中的搜索、98. 验证二叉搜索树"
date: 2026-03-20
tags: [二叉树, 二叉搜索树, DFS]
---

## 654. 最大二叉树

- 链接：[654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0654.最大二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1MG411G7ox)
- 状态：✅

### 第一想法
和LC105, 106一样, 但是逻辑更加简单,直接采用传index;

### 看完题解后的想法
--

### 实现中遇到的困难
注意index是全局index所以直接传

### 代码

```cpp
class Solution {
public:
    TreeNode* recurse(vector<int>& nums, int begin, int end) {
        if (begin == end) return nullptr;

        // 1. 找到max值和index，这就是root
        int indexRoot = begin;
        for (int i = begin; i < end; i++) {
            if (nums[i] > nums[indexRoot]) {
                indexRoot = i;
            }
        }
        TreeNode* root = new TreeNode(nums[indexRoot]);

        // 2. 挂树+直接传index做分组，注意index是全局index所以直接传
        root->left = recurse(nums, begin, indexRoot);
        root->right = recurse(nums, indexRoot+1, end);

        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return recurse(nums, 0, (int)nums.size());
    }
};
```

## 617. 合并二叉树

- 链接：[617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)
- 文章：[代码随想录](https://programmercarl.com/0617.合并二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1m14y1Y7JK)
- 状态：⚠️

### 第一想法
使用递归， 返回 root1 ， 只要 root1 没有某棵树， 就挂上 root2 的；

### 看完题解后的想法
可以简化：核心逻辑就是
```cpp
if (!root1) return root2;
if (!root2) return root1;
```
如果root1没有，就返回root2（挂上），如果root2没有，那直接返回root1；

### 实现中遇到的困难
--

### 代码
简化版：
```cpp
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (!root1) return root2;
        if (!root2) return root1;

        root1->val += root2->val;
        root1->left = mergeTrees(root1->left, root2->left);
        root1->right = mergeTrees(root1->right, root2->right);
        return root1;
    }
};
```
自己写的复杂版：
```cpp
class Solution {
public:
    void dfs(TreeNode* root1, TreeNode* root2) {
        if (!root2) return;

        root1->val = root1->val + root2->val;

        if (!root1->left) {
            root1->left = root2->left;
        } else {
            dfs(root1->left, root2->left);
        }

        if (!root1->right) {
            root1->right = root2->right;
        } else {
            dfs(root1->right, root2->right);
        }
    }

    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (!root1) return root2;
        if (!root2) return root1;
        dfs(root1, root2);
        return root1;
    }
};
```

## 700. 二叉搜索树中的搜索

- 链接：[700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0700.二叉搜索树中的搜索.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1wG411g7sF)
- 状态：✅

### 第一想法
利用二叉搜索树的性质进行大小判断；

### 看完题解后的想法
利用二叉搜索树的性质直接进行迭代更简单，不用额外的空间，直接跑；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root) {
            if (root->val == val) {
                return root;
            } else if (root->val > val) {
                root = root->left;
            } else {
                root = root->right;
            }
        }

        return nullptr;
    }
};
```

## 98. 验证二叉搜索树

- 链接：[98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0098.验证二叉搜索树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV18P411n7Q4)
- 状态：✅

### 第一想法
利用中序遍历 `左 - 根 - 右` 进行比较；

### 看完题解后的想法
可以转换为一个序列，但是递归可以直接做，只是要多一个全局变量用作记录上一个值；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    long long prev = LLONG_MIN;

    bool isValidBST(TreeNode* root) {
        if (!root) {
            return true;
        }

        bool left = isValidBST(root->left);

        if (root->val <= prev) {
            return false;
        }
        prev = root->val;

        bool right = isValidBST(root->right);

        return left && right;
    }
};
```

## 今日收获
