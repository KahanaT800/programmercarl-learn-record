---
title: "代码随想录打卡第18天 | 530. 二叉搜索树的最小绝对差、501. 二叉搜索树中的众数、236. 二叉树的最近公共祖先"
date: 2026-03-21
tags: [二叉树, 二叉搜索树, DFS]
---

## 530. 二叉搜索树的最小绝对差

- 链接：[530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)
- 文章：[代码随想录](https://programmercarl.com/0530.二叉搜索树的最小绝对差.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1DD4y11779)
- 状态：⚠️

### 第一想法
中序遍历是有序的，直接用当前减过去；

### 看完题解后的想法
--

### 实现中遇到的困难
需要对最左节点进行一个特判，因为最左节点没有可以相减的；

### 代码

```cpp
class Solution {
public:
    int res = INT_MAX;
    int prev = -1;  // -1 表示还没访问过任何节点

    void dfs(TreeNode* root) {
        if (!root) return;

        dfs(root->left);

        if (prev != -1) {
            res = min(res, root->val - prev);  // BST中序是升序，不需要abs
        }
        prev = root->val;

        dfs(root->right);
    }

    int getMinimumDifference(TreeNode* root) {
        dfs(root);
        return res;
    }
};
```

## 501. 二叉搜索树中的众数

- 链接：[501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0501.二叉搜索树中的众数.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1fD4y117gp)
- 状态：⚠️

### 第一想法
遍历+哈希，直接在递归中怎么做不知道；

### 看完题解后的想法
最关键的一步就是更新count和max，一旦发现有更大的count就重置max，清空res；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int max = 0;
    int count = 0;
    vector<int> res;
    TreeNode* prev = nullptr;

    void dfs(TreeNode* root) {
        if (!root) return;

        dfs(root->left);

        // 计数部分
        if (!prev) {
            count = 1;
        } else if (root->val == prev->val) {
            count++;
        } else {
            count = 1;
        }
        prev = root;

        if (count == max) {
            res.push_back(root->val);
        } else if (count > max) { // 关键：如果当前count更大就清空res
            max = count;
            res.clear();
            res.push_back(root->val);
        }

        dfs(root->right);
    }

    vector<int> findMode(TreeNode* root) {
        dfs(root);
        return res;
    }
};
```

## 236. 二叉树的最近公共祖先

- 链接：[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0236.二叉树的最近公共祖先.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1jd4y1B7E2)
- 状态：⚠️

### 第一想法
终止条件：root 为空说明没找到；root == p 或 root == q 说明找到了目标，直接返回。

递归：分别去左右子树找。

处理返回值，三种情况：
- left 和 right 都不为空 → p 和 q 分别在左右两边，当前 root 就是最近公共祖先，返回 root
- 只有一边不为空 → p 和 q 都在那一边，把那边的结果往上传
- 都为空 → 这棵子树里没有 p 和 q，返回 nullptr

```text
        3
       / \
      5   1
     / \
    6   2

lowestCommonAncestor(3, 5, 6)
├── left = lowestCommonAncestor(5, 5, 6)
│   → root == p，直接返回5，不再往下递归
│
├── right = lowestCommonAncestor(1, 5, 6)
│   → 最终返回nullptr
│
└── 只有left不空 → 返回5
```

既然 6 存在于树中，而我们在节点 5 就找到了 p，6 不在右子树（右子树返回了 nullptr），也不在 5 的上方（5 是从左子树返回的），那 6 只可能在 5 的子树里。所以 5 本身就是公共祖先，不需要再往下验证。

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q) {
            return root;
        }

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        // left 和 right 都找到了，说明当前 root 就是最近公共祖先
        if (left != nullptr && right != nullptr) {
            return root;
        }

        // 如果只有一边找到了，说明公共祖先在那一支中
        return left ? left : right;
    }
};
```

## 今日收获
