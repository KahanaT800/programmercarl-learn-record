---
title: "代码随想录打卡第15天 | 110. 平衡二叉树、257. 二叉树的所有路径、404. 左叶子之和、222. 完全二叉树的节点个数"
date: 2026-03-18
tags: [二叉树, DFS]
---

## 110. 平衡二叉树

- 链接：[110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0110.平衡二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Ug411S7my)
- 状态：❌

### 第一想法
没想出来;

### 看完题解后的想法
从底到顶递归算高度,很巧妙;

对于底层叶子往上递归的过程需要理解;

### 实现中遇到的困难


### 代码

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return getHigh(root) != -1;
    }

    int getHigh(TreeNode* root) {
        // 1. 递归到底之后返回 0
        if (!root) {
            return 0;
        }

        // 2. 左子树高度
        int left = getHigh(root->left);
        if (left == -1) return -1;
        // 3. 右子树高度
        int right = getHigh(root->right);
        if (right == -1) return -1;

        if (abs(left - right) > 1) {
            return -1;
        }

        // 4. 返回当前高度,注意因为叶子节点的左右子树都是 nullptr 返回了 0,高度为 1
        return 1 + max(left, right);
    }
};
```

## 257. 二叉树的所有路径

- 链接：[257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)
- 文章：[代码随想录](https://programmercarl.com/0257.二叉树的所有路径.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ZG411G7Dh)
- 状态：✅

### 第一想法
使用递归，一层加一个路径；

### 看完题解后的想法
可以传引用+手动回溯，但是AC没必要；

### 实现中遇到的困难
1. 注意返回时机，不要等走到nullptr返回路径，因为这样会返回两次；
2. 注意字符串不能使用引用，利用值传递复制的性质每一个递归栈享有一个独立的路径，免去了需要回溯的麻烦；

### 代码

```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        string s;
        dfs(root, res, s);
        return res;
    }

    void dfs(TreeNode* root, vector<string>& res, string s) {
        if (!root) {
            return;
        }

        s += to_string(root->val);
        if (!root->left && !root->right) {
            res.push_back(s);
            return;
        }
        s += "->";
        dfs(root->left, res, s);
        dfs(root->right, res, s);
    }
};
```

## 404. 左叶子之和

- 链接：[404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)
- 文章：[代码随想录](https://programmercarl.com/0404.左叶子之和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1K7z8)
- 状态：✅

### 第一想法
DFS，带上方向，直接使用 int& sum 在递归中加值

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        int sum = 0;
        dfs(root, sum, false);
        return sum;
    }

    void dfs(TreeNode* root, int& sum, bool isLeft) {
        if (!root) {
            return;
        }

        if (!root->left && !root->right && isLeft) {
            sum += root->val;
            return;
        }

        dfs(root->left, sum, true);
        dfs(root->right, sum, false);
    }
};
```

## 222. 完全二叉树的节点个数

- 链接：[222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)
- 文章：[代码随想录](https://programmercarl.com/0222.完全二叉树的节点个数.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1eW4y1B7pD)
- 状态：⚠️

### 第一想法
层序遍历来做,但是优化方案不会;

### 看完题解后的想法
充分利用了完全二叉树的性质,如果一棵树是完全二叉树则节点数可以直接用公式;

核心就是递归去看一个节点的左右子树高度是否一致,完全二叉树的左右子树高度一致,如果高度一致，则可以知道这是个完全二叉树，直接使用层高来算节点个数; 如果不一致则进左右子树分别看；

最后递归到叶子节点也会返回一个1;(左右子树都是nullptr)

### 实现中遇到的困难


### 代码

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) {
            return 0;
        }
        TreeNode* leftTree = root->left;
        TreeNode* rightTree = root->right;
        int leftH = 0, rightH = 0;
        // 左树高度
        while (leftTree) {
            leftTree = leftTree->left;
            leftH++;
        }
        // 右树高度
        while (rightTree) {
            rightTree = rightTree->right;
            rightH++;
        }
        // 高度一致，完全二叉树
        if (leftH == rightH) {
            return (2<<leftH)-1;
        }
        // 不一致，进下一层递归
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```

## 今日收获
