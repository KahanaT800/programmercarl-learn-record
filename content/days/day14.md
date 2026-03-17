---
title: "代码随想录打卡第14天 | 226. 翻转二叉树、101. 对称二叉树、104. 二叉树的最大深度、111. 二叉树的最小深度"
date: 2026-03-17
tags: [二叉树, DFS, BFS]
---

## 226. 翻转二叉树

- 链接：[226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0226.翻转二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1sP4y1f7q7)
- 状态：✅

### 第一想法
遍历节点，然后交换节点的左右子树，DFS和BFS都能做；

### 看完题解后的想法
注意中序遍历因为是 `左-根-右` ，在根节点交换完之后要继续访问左子树，因为已经交换了；

### 实现中遇到的困难
中序和后序的迭代写法还是记不牢；

### 代码
前序递归解：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        dfs(root);
        return root;
    }

    void dfs(TreeNode* root) {
        if (!root) return;

        TreeNode* tmp = root->right;
        root->right = root->left;
        root->left = tmp;

        dfs(root->left);
        dfs(root->right);
    }
};
```
中序递归解：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        dfs(root);
        return root;
    }

    void dfs(TreeNode* root) {
        if (!root) return;

        dfs(root->left);
        TreeNode* tmp = root->right;
        root->right = root->left;
        root->left = tmp;
        dfs(root->left); // 交换左右所以记得继续走左
    }
};
```
前序迭代：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) {
            return root;
        }
        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();

            TreeNode* tmp = cur->right;
            cur->right = cur->left;
            cur->left = tmp;

            if (cur->right) st.push(cur->right);
            if (cur->left) st.push(cur->left);
        }
        return root;
    }
};
```
中序迭代：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) {
            return root;
        }
        stack<TreeNode*> st;
        TreeNode* cur = root;

        while (cur || !st.empty()) {
            while (cur) {
                st.push(cur);
                cur = cur->left;
            }
            cur = st.top();
            st.pop();

            TreeNode* tmp = cur->right;
            cur->right = cur->left;
            cur->left = tmp;

            cur = cur->left;
        }
        return root;
    }
};
```
后序迭代：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) {
            return root;
        }
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* visited = nullptr;

        while (cur || !st.empty()) {
            while (cur) {
                st.push(cur);
                cur = cur->left;
            }
            TreeNode* top = st.top();
            if (top->right && top->right != visited) {
                cur = top->right;
            } else {
                st.pop();
                TreeNode* tmp = top->right;
                top->right = top->left;
                top->left = tmp;
                visited = top;
            }
        }
        return root;
    }
};
```
层序：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) {
            return root;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = q.front();
                q.pop();
                TreeNode* tmp = cur->right;
                cur->right = cur->left;
                cur->left = tmp;

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }

        }
        return root;
    }
};
```

## 101. 对称二叉树

- 链接：[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)
- 文章：[代码随想录](https://programmercarl.com/0101.对称二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ue4y1Y7Mf)
- 状态：❌

### 第一想法
第一想法用层序遍历，但是就发现控制节点进出特别麻烦，而且因为层序遍历不知道顺序，还需要给每个节点加一个方向组成pair；

### 看完题解后的想法
直接递归，比较就比较内侧和外侧是否对称就可以了；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return check(root->left, root->right);
    }

    bool check(TreeNode* t1, TreeNode* t2) {
        if (!t1 && !t2) return true;
        if (!t1 || !t2) return false;

        return t1->val == t2->val && check(t1->left, t2->right) && check(t1->right, t2->left);
    }
};
```

## 104. 二叉树的最大深度

- 链接：[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0104.二叉树的最大深度.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Gd4y1V75u)
- 状态：✅

### 第一想法
层序遍历，遍历一层数一层；

### 看完题解后的想法
需要掌握递归解；

### 实现中遇到的困难
--

### 代码
递归解：
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        // nullptr为 0， 那么返回的叶子节点层高就是 1， 然后依次往上叠加
        if (!root) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```
层序遍历迭代解：
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        int cnt = 0;
        if (!root) {
            return cnt;
        }
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = q.front();
                q.pop();

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            cnt++;
        }

        return cnt;
    }
};
```

## 111. 二叉树的最小深度

- 链接：[111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0111.二叉树的最小深度.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1QD4y1B7e2)
- 状态：✅

### 第一想法
在层序遍历的时候加一个判断，如果有节点是叶子节点直接返回当前层数；

### 看完题解后的想法
需要掌握递归解；

最小深度的定义：根节点到最近叶子节点的路径长度。叶子节点是左右子树都为空的节点。

三种情况：
- 左子树空、右子树不空：最小深度只能从右边找，返回 1 + rightDepth
- 左子树不空、右子树空：最小深度只能从左边找，返回 1 + leftDepth
- 两边都不空：取较小的，返回 1 + min(leftDepth, rightDepth)

### 实现中遇到的困难
--

### 代码
递归解：
```cpp
class Solution {
public:
    int getDepth(TreeNode* root) {
        if (!root) return 0;
        int leftDepth = getDepth(root->left);
        int rightDepth = getDepth(root->right);

        if (!root->left && root->right) {
            return 1 + rightDepth;
        }

        if (root->left && !root->right) {
            return 1 + leftDepth;
        }

        return 1 + min(leftDepth, rightDepth);
    }
    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```
层序迭代解：
```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        int cnt = 0;
        if (!root) {
            return cnt;
        }
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            int size = q.size();
            cnt++;
            for (int i = 0; i < size; i++) {
                TreeNode* cur = q.front();
                q.pop();

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
                if (!(cur->left) && !(cur->right)) {
                    return cnt;
                }
            }
        }

        return cnt;
    }
};
```

## 今日收获
