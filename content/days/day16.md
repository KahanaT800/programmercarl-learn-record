---
title: "代码随想录打卡第16天 | 513. 找树左下角的值、112. 路径总和、113. 路径总和II、106. 从中序与后序遍历序列构造二叉树、105. 从前序与中序遍历序列构造二叉树"
date: 2026-03-19
tags: [二叉树, DFS, BFS]
---

## 513. 找树左下角的值

- 链接：[513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)
- 文章：[代码随想录](https://programmercarl.com/0513.找树左下角的值.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1424y1Z7pn)
- 状态：✅

### 第一想法
层序遍历，每层的第一个值

### 看完题解后的想法


### 实现中遇到的困难


### 代码

```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        int res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int num = q.size();
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();
                if (i == 0) {
                    res = cur->val;
                }

                if (cur->left) {
                    q.push(cur->left);
                }
                if (cur->right) {
                    q.push(cur->right);
                }
            }
        }

        return res;
    }
};
```

## 112. 路径总和

- 链接：[112. 路径总和](https://leetcode.cn/problems/path-sum/)
- 文章：[代码随想录](https://programmercarl.com/0112.路径总和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV19t4y1L7CR)
- 状态：✅

### 第一想法
DFS+回溯；栈自带回溯；

### 看完题解后的想法
可以回溯，但没必要；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        int sum = 0;
        return dfs(root, targetSum, sum);
    }

    bool dfs(TreeNode* root, int target, int sum) {
        sum += root->val;
        if (!root->left && !root->right) {
            return sum == target;
        }

        if (root->left && dfs(root->left, target, sum)) return true;
        if (root->right && dfs(root->right, target, sum)) return true;

        return false;
    }
};
```

## 113. 路径总和II

- 链接：[113. 路径总和II](https://leetcode.cn/problems/path-sum-ii/)
- 文章：[代码随想录](https://programmercarl.com/0112.路径总和.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV19t4y1L7CR)
- 状态：✅

### 第一想法
和112一样，只是收集所有满足条件的路径，而不是返回bool；

### 看完题解后的想法
--

### 实现中遇到的困难
用值传递传 path 的话每一层栈有独立副本，免去手动回溯；但会有额外的拷贝开销，用引用+手动回溯更省空间；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> res;
        if (!root) return res;
        vector<int> path;
        dfs(root, targetSum, 0, path, res);
        return res;
    }

    void dfs(TreeNode* root, int target, int sum, vector<int>& path, vector<vector<int>>& res) {
        sum += root->val;
        path.push_back(root->val);

        if (!root->left && !root->right) {
            if (sum == target) {
                res.push_back(path);
            }
            path.pop_back(); // 回溯
            return;
        }

        if (root->left) dfs(root->left, target, sum, path, res);
        if (root->right) dfs(root->right, target, sum, path, res);

        path.pop_back(); // 回溯
    }
};
```

## 106. 从中序与后序遍历序列构造二叉树

- 链接：[106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- 文章：[代码随想录](https://programmercarl.com/0106.从中序与后序遍历序列构造二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1vW4y1i7dn)
- 状态：❌

### 第一想法
没有想法,对于二叉树遍历后序列的特点没有印象;

### 看完题解后的想法
前序遍历: `根 - 左 - 右`

中序遍历: `左 - 根 - 右`

后序遍历: `左 - 右 - 根`

关键点：后序遍历的最后一位一定是当前的root，中序的root左右两边自动就是左右子树;

依靠这个不断地去切两个序列；

### 实现中遇到的困难
要对切序列时的序号保持敏感，要保持不变量，这里使用左闭右开；（因为 `.end()` 迭代器指向的不是末尾元素而是末尾元素的后一位，天生就是开区间）

### 代码

```cpp
class Solution {
public:
    TreeNode* recerse(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.size() == 0) return nullptr;
        // 1. 找到后序遍历的最后一个数，这个数就是当前的root
        int rootVal = postorder[(int)postorder.size()-1];
        TreeNode* root = new TreeNode(rootVal); // new返回的是指针
        // 如果后序序列size为1，说明已经切到叶子节点，该返回了
        if (postorder.size() == 1) return root;

        // 2. 找到根节点在中序序列的位置，从该位置分为左子树和右子树
        int indexRoot;
        for (int i = 0; i < (int)inorder.size(); i++) {
            if (rootVal == inorder[i]) {
                indexRoot = i;
            }
        }

        // 3. 切中序为左右子树
        vector<int> leftInOrder(inorder.begin(), inorder.begin()+indexRoot);
        vector<int> rightInOrder(inorder.begin()+indexRoot+1, inorder.end());

        // 4. 切后序为左右子树
        // 去掉末尾元素
        postorder.resize((int)postorder.size()-1);
        // 后序序列的子树大小和中序序列切出来的一样
        vector<int> leftPostOrder(postorder.begin(), postorder.begin()+(int)leftInOrder.size());
        vector<int> rightPostOrder(postorder.begin()+(int)leftInOrder.size(), postorder.end());

        // 5. 挂树
        root->left = recerse(leftInOrder, leftPostOrder);
        root->right = recerse(rightInOrder, rightPostOrder);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return recerse(inorder, postorder);
    }
};
```

## 105. 从前序与中序遍历序列构造二叉树

- 链接：[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- 文章：[代码随想录](https://programmercarl.com/0106.从中序与后序遍历序列构造二叉树.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1vW4y1i7dn)
- 状态：⚠️

### 第一想法
和LC106一样,两个序列互相切;

### 看完题解后的想法
如果使用序号法而不是新建vector，首元素不能硬编码为 `0`，而是要使用传入的 `index`；

### 实现中遇到的困难
前序要切掉首元素，所以构造新vector的时候应该从 `begin()+1` 开始，特别注意右边界也要记得 `+1`；

### 代码

```cpp
class Solution {
public:
    TreeNode* recerse(vector<int>& inorder, vector<int>& preorder) {
        if (preorder.size() == 0) return nullptr;
        // 1. 找到前序遍历的第一个数，这个数就是当前的root
        int rootVal = preorder[0];
        TreeNode* root = new TreeNode(rootVal); // new返回的是指针
        // 如果前序序列size为1，说明已经切到叶子节点，该返回了
        if (preorder.size() == 1) return root;

        // 2. 找到根节点在中序序列的位置，从该位置分为左子树和右子树
        int indexRoot;
        for (int i = 0; i < (int)inorder.size(); i++) {
            if (rootVal == inorder[i]) {
                indexRoot = i;
            }
        }

        // 3. 切中序为左右子树
        vector<int> leftInOrder(inorder.begin(), inorder.begin()+indexRoot);
        vector<int> rightInOrder(inorder.begin()+indexRoot+1, inorder.end());

        // 4. 切前序为左右子树，注意要切掉首元素，所以开始是begin()+1
        // 前序序列的子树大小和中序序列切出来的一样
        vector<int> leftPreOrder(preorder.begin()+1, preorder.begin()+1+(int)leftInOrder.size());
        vector<int> rightPreOrder(preorder.begin()+1+(int)leftInOrder.size(), preorder.end());

        // 5. 挂树
        root->left = recerse(leftInOrder, leftPreOrder);
        root->right = recerse(rightInOrder, rightPreOrder);

        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return recerse(inorder, preorder);
    }
};
```

## 今日收获
