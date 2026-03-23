---
title: "代码随想录打卡第20天 | 235. 二叉搜索树的最近公共祖先、701. 二叉搜索树中的插入操作、450. 删除二叉搜索树中的节点"
date: 2026-03-23
tags: [二叉树, 二叉搜索树]
---

## 235. 二叉搜索树的最近公共祖先

- 链接：[235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0235.二叉搜索树的最近公共祖先.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Zt4y1F7ww)
- 状态：⚠️

### 第一想法
利用二叉搜索树的性质，可以用数值大小确定子树，**第一次**遇到的在 [p, q] 或者 [q, p] 间的数，就是最近公共祖先；

BST 中，对于当前节点 cur，有三种情况：
- 情况一：cur 的值比 p 和 q 都大。说明 p 和 q 都在 cur 的左子树里，公共祖先一定在左边，cur 不是答案。
- 情况二：cur 的值比 p 和 q 都小。同理，都在右子树，往右走。
- 情况三：cur 的值在 p 和 q 之间（含等于）。这时候只有两种可能：

**p 和 q 分别在 cur 的左右子树 → cur 就是公共祖先**

cur 本身就是 p 或 q 中的一个，另一个在它的子树里 → cur 也是公共祖先

### 看完题解后的想法
--

### 实现中遇到的困难
需要确定 p 和 q 的大小关系；

### 代码

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val > q->val) swap(p, q);
        TreeNode* cur = root;
        while (cur) {
            if (cur->val >= p->val && cur->val <= q->val) {
                return cur;
            } else if (cur->val < p->val) {
                cur = cur->right;
            } else {
                cur = cur->left;
            }
        }
        return nullptr;
    }
};
```

## 701. 二叉搜索树中的插入操作

- 链接：[701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)
- 文章：[代码随想录](https://programmercarl.com/0701.二叉搜索树中的插入操作.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Et4y1c78Y)
- 状态：⚠️

### 第一想法
二叉搜索树，可以直接比大小前进；然后找到空位就可以插进去了；

### 看完题解后的想法
--

### 实现中遇到的困难
注意不是一定要插在叶子节点上，中间的节点有空位且满足条件就能直接插入；

这里特别要理解的就是为什么有空位就可以直接插入：
BST 插入的位置是唯一确定的。从根节点开始比大小往下走，路径是固定的，走到空位就是唯一合法的插入点。

### 代码

```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) return new TreeNode(val);

        TreeNode* cur = root;
        while (cur) {
            if (cur->val > val) {
                // 要去比较的地方没有节点，说明这个位置就是该待的地方
                if (!cur->left) {
                    TreeNode* node = new TreeNode(val);
                    cur->left = node;
                    return root;
                }
                cur = cur->left;
            } else {
                if (!cur->right) {
                    TreeNode* node = new TreeNode(val);
                    cur->right = node;
                    return root;
                }
                cur = cur->right;
            }
        }
        return root;
    }
};
```

## 450. 删除二叉搜索树中的节点

- 链接：[450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)
- 文章：[代码随想录](https://programmercarl.com/0450.删除二叉搜索树中的节点.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1tP41177us)
- 状态：⚠️

### 第一想法
对需要删除的节点和左右子树情况进行分析，每一种情况单独处理；

### 看完题解后的想法
要充分利用 BST 的性质，key 如果大了，就说明一定在当前节点的右边，那返回的节点就可以直接接在当前节点的右边，这样就不需要判断左右，也不需要多设置一个 prev，也就不需要 dummy 了；

### 实现中遇到的困难
1. 删除叶子结点之后要让 prev 的指针指向 nullptr，不然就是悬空指针；
2. 引入虚拟头节点，简化了头节点的处理逻辑

### 代码

自己写的土味方法：
```cpp
class Solution {
public:
    void dfs(TreeNode* root, int key, TreeNode* prev, bool isLeft) {
        if (!root) {
            return;
        }

        TreeNode* cur = root;
        if (cur->val == key) {
            if (!cur->left && !cur->right) {
                // 如果该节点为叶子节点，直接删除
                if (isLeft) {
                    prev->left = nullptr;
                } else {
                    prev->right = nullptr;
                }
                delete cur;
                return;
            } else if (cur->left && !cur->right) {
                // 当前节点只有左树
                if (isLeft) {
                    prev->left = cur->left;
                } else {
                    prev->right = cur->left;
                }
                delete cur;
                return;
            } else if (!cur->left && cur->right) {
                // 当前节点只有右树
                if (isLeft) {
                    prev->left = cur->right;
                } else {
                    prev->right = cur->right;
                }
                delete cur;
                return;
            } else {
                // 当前节点有左右子树
                // 把右子树挂到当前位置，把左子树移到右子树的最左节点的左边
                TreeNode* right = cur->right;
                TreeNode* left = cur->left;
                if (isLeft) {
                    prev->left = right;
                } else {
                    prev->right = right;
                }
                // 找到右子树的最左节点
                TreeNode* tmp = right;
                TreeNode* tmpPrev = nullptr;
                while (tmp) {
                    tmpPrev = tmp;
                    tmp = tmp->left;
                }
                tmpPrev->left = left;

                delete cur;
                return;
            }
        }

        // 当前节点不需要删除，递归
        prev = root;
        if (key < root->val) {
            dfs(root->left, key, prev, true);
        } else if (key > root->val) {
            dfs(root->right, key, prev, false);
        }
    }

    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) {
            return nullptr;
        }
        // 引入虚拟头节点，让根节点也拥有 prev
        TreeNode dummy(0);
        dummy.left = root;

        dfs(dummy.left, key, &dummy, true);

        return dummy.left;
    }
};
```

充分利用 BST 性质的递归解：
```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) {
            return nullptr;
        }

        if (key < root->val) {
            // 直接把返回的树挂在相应位置
            root->left = deleteNode(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNode(root->right, key);
        } else {
            if (!root->left && !root->right) {
                // 叶子节点，直接删除
                delete root;
                return nullptr;
            } else if (root->left && !root->right) {
                // 只有左树
                TreeNode* left = root->left;
                delete root;
                return left;
            } else if (!root->left && root->right) {
                // 只有右树
                TreeNode* right = root->right;
                delete root;
                return right;
            } else {
                // 左右子树都有：把右子树挂到当前位置，把左子树移到右子树的最左节点的左边
                TreeNode* right = root->right;
                TreeNode* left = root->left;
                // 找到右子树的最左节点
                TreeNode* tmp = right;
                TreeNode* tmpPrev = nullptr;
                while (tmp) {
                    tmpPrev = tmp;
                    tmp = tmp->left;
                }
                tmpPrev->left = left;

                delete root;
                return right;
            }
        }

        return root;
    }
};
```

## 今日收获
