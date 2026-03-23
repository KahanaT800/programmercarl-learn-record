---
title: "代码随想录打卡第21天 | 669. 修剪二叉搜索树、108. 将有序数组转换为二叉搜索树、538. 把二叉搜索树转换为累加树"
date: 2026-03-24
tags: [二叉树, 二叉搜索树, DFS]
---

## 669. 修剪二叉搜索树

- 链接：[669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0669.修剪二叉搜索树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV17P41177ud)
- 状态：❌

### 第一想法
在小于low的节点，节点和左树全部抛掉，返回右树（如果右树不存在一样返回）；

大于high的节点，节点和右树全部抛掉，返回左树（如果左树不存在一样返回）；

### 看完题解后的想法
要学会运用返回节点简化操作，这题和LC450类似，都是用返回节点简化了左右判断，因为BST自带左右；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;

        if (root->val < low) {
            return trimBST(root->right, low, high);
        }
        if (root->val > high) {
            return trimBST(root->left, low, high);
        }

        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);

        return root;
    }
};
```

## 108. 将有序数组转换为二叉搜索树

- 链接：[108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0108.将有序数组转换为二叉搜索树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1uR4y1X7qL)
- 状态：✅

### 第一想法
不断取中点构造二叉树;

### 看完题解后的想法
--

### 实现中遇到的困难
1. 递归终止条件要使用下标
2. mid 是绝对坐标，所以传递坐标时要直接传 mid 而不是偏移;

### 代码

```cpp
class Solution {
public:
    TreeNode* recurse(vector<int>& nums, int left, int right) {
        if (right - left == 0) return nullptr;

        int mid = left + (right - left) / 2;
        TreeNode* node = new TreeNode(nums[mid]);

        node->left = recurse(nums, left, mid);
        node->right = recurse(nums, mid+1, right);
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root = recurse(nums, 0, (int)nums.size());
        return root;
    }
};
```

## 538. 把二叉搜索树转换为累加树

- 链接：[538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)
- 文章：[代码随想录](https://programmercarl.com/0538.把二叉搜索树转换为累加树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1d44y1f7wP)
- 状态：✅

### 第一想法
第一个想法是用外部 vector 存中序结果然后倒着改一遍；

第二个想法是两次中序，一遍得到 sum，一遍得到新树；

想着如果能反中序递归就很简单；

### 看完题解后的想法
把中序的左右递归位置对调就是反中序递归；

### 实现中遇到的困难
--

### 代码

一次递归（反中序）：
```cpp
class Solution {
public:
    void dfs(TreeNode* root, int& sum) {
        if (!root) return;
        dfs(root->right, sum);
        sum += root->val;
        root->val = sum;
        dfs(root->left, sum);
    }

    TreeNode* convertBST(TreeNode* root) {
        int sum = 0;
        dfs(root, sum);
        return root;
    }
};
```

两次递归：
```cpp
class Solution {
public:
    void getSum(TreeNode* root, int& sum) {
        if (!root) return;
        getSum(root->left, sum);
        sum += root->val;
        getSum(root->right, sum);
    }
    void newTree(TreeNode* root, int& sum) {
        if (!root) return;
        newTree(root->left, sum);
        int tmp = root->val;
        root->val = sum;
        sum -= tmp;
        newTree(root->right, sum);
    }

    TreeNode* convertBST(TreeNode* root) {
        int sum = 0;
        getSum(root, sum);
        newTree(root, sum);
        return root;
    }
};
```

中序遍历 + 逆序更新：
```cpp
class Solution {
public:
    void dfs(TreeNode* root, vector<TreeNode*>& vt) {
        if (!root) return;
        dfs(root->left, vt);
        vt.push_back(root);
        dfs(root->right, vt);
    }
    TreeNode* convertBST(TreeNode* root) {
        vector<TreeNode*> vt;
        dfs(root, vt);
        int sum = 0;
        for (int i = (int)vt.size() - 1; i >= 0; i--) {
            sum += vt[i]->val;
            vt[i]->val = sum;
        }
        return root;
    }
};
```

## 今日收获
