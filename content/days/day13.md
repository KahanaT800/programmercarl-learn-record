---
title: "代码随想录打卡第13天 | 二叉树理论基础、递归遍历、迭代遍历、层序遍历十题"
date: 2026-03-16
tags: [二叉树, DFS, BFS]
---

需要了解二叉树的种类，存储方式，遍历方式以及二叉树的定义。

- 文章：[代码随想录 - 二叉树理论基础](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

# DFS

## 144. 二叉树的前序遍历

- 链接：[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
- 文章：[代码随想录](https://programmercarl.com/二叉树的递归遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Wh411S7xt)
- 状态：✅

### 第一想法
根 - 左 - 右

可以递归dfs，还可以迭代用栈模拟，还可以用Morris，但是Morris太复杂了；

### 看完题解后的想法
前序是"根-左-右"，弹出就访问，访问完把左右子节点压栈。节点被弹出只有一种含义：该访问了。不存在"我还要回来"的情况。

### 实现中遇到的困难
注意压栈的顺序是后访问的先压;

### 代码
递归解：(爆栈风险)
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }

    void dfs(TreeNode* root, vector<int>& res) {
        if (root == nullptr) {
            return ;
        }
        res.push_back(root->val);
        dfs(root->left, res);
        dfs(root->right, res);
    }
};
```
迭代解：(面试要求)
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
        }
        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();

            res.push_back(cur->val);
            // 先压后访问的
            if (cur->right) {
                st.push(cur->right);
            }
            // 再压先访问的
            if (cur->left) {
                st.push(cur->left);
            }
        }
        return res;
    }
};
```

## 145. 二叉树的后序遍历

- 链接：[145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
- 文章：[代码随想录](https://programmercarl.com/二叉树的递归遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Wh411S7xt)
- 状态：❌

### 第一想法
左 - 右 - 根, 迭代解不会;

### 看完题解后的想法
后序遍历的迭代解和中序的迭代类似,都是用了一路向左压栈,让压入顺序自动变成 左 - 根;

但是后序的不同点在于是 左 -右 - 根, 因此需要判断所在的节点是不是中间的根节点, 如果是需要先去右子树;

最后弹栈的顺序就变成了 右 - 根, 其实就是在中序的 左 - 根 的顺序上上插入 右;

注意开始弹栈之后需要标记当前的访问位置

后序要求访问顺序是"左-右-根"，所以一个节点从栈顶被看到时，你面临一个问题——我是该去右子树，还是该访问自己？
```
        top
       /   \
     左     右
```

栈顶是 top，你刚从左子树回来和刚从右子树回来，看到的栈状态是一模一样的。没有任何信息能告诉你左子树和右子树哪个已经处理完了。prev 就是用来解决这个歧义的——如果 prev == top->right，说明右子树刚访问完，该访问自己了。

### 实现中遇到的困难
进入右子树不是压入右节点;

### 代码
递归解：
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }

    void dfs(TreeNode* root, vector<int>& res) {
        if (root == nullptr) {
            return ;
        }
        dfs(root->left, res);
        dfs(root->right, res);
        res.push_back(root->val);
    }
};
```
迭代解：
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
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
            // 如果右子树存在且没被访问过，进入右子树
            if (top->right && top->right != visited) {
                cur = top->right;
            } else {
                st.pop();
                res.push_back(top->val);
                visited = top;
            }
        }

        return res;
    }
};
```

## 94. 二叉树的中序遍历

- 链接：[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
- 文章：[代码随想录](https://programmercarl.com/二叉树的递归遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Wh411S7xt)
- 状态：⚠️

### 第一想法
左 - 根 -右; 面试要考迭代解

### 看完题解后的想法
中序是"左-根-右"，节点从栈里弹出时，左子树一定已经处理完了（因为内层 while 已经走到最左底部了），所以弹出就意味着该访问自己，然后 curr = curr->right 转向右子树。右子树是通过 curr 这个外部指针去探索的，不会再回到当前节点。节点弹出后就从栈里消失了，不存在"再次看到同一个节点"的情况。

### 实现中遇到的困难
理解为什么一路向左压, 弹出的时候顺序自动就是左 - 根, 最后再压入右;

### 代码
递归解：
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }

    void dfs(TreeNode* root, vector<int>& res) {
        if (root == nullptr) {
            return ;
        }
        dfs(root->left, res);
        res.push_back(root->val);
        dfs(root->right, res);
    }
};
```
迭代解：
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
        }
        stack<TreeNode*> st;
        TreeNode* cur = root;

        while (cur || !st.empty()) {
            // 一路向左压
            while (cur) {
                st.push(cur);
                cur = cur->left;
            }
            // 到底弹出（弹出顺序一定是 左 - 根）
            cur = st.top();
            st.pop();
            res.push_back(cur->val);
            // 最后转右
            cur = cur->right;
        }

        return res;
    }
};
```

学有余力可以掌握统一迭代法的写法。

- 文章：[代码随想录](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html)

# BFS

## 102. 二叉树的层序遍历

- 链接：[102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
需要用队列来装每一层的节点；因为题目要求是一层一层的输出，所以难点就是如何知道这是一层的数据；

最开始的想法是用两个队列，一个队列是临时队列存下一层的节点，在本层空了再之后swap进去；

### 看完题解后的想法
其实queue自带size，可以记录每一层的size，然后用for循环遍历每一层，这样即使是queue中同时存在本层和下一层的节点也没关系；

空间复杂度和时间复杂度是一样的，只是少维护了一个变量；

### 实现中遇到的困难
最后把每层的数据放进二维矩阵中要在遍历结束后；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            vector<int> r;
            int num = q.size(); // 记录当前层的节点数
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();
                r.push_back(cur->val);

                if (cur->left) {
                    q.push(cur->left);
                }
                if (cur->right) {
                    q.push(cur->right);
                }
            }
            res.push_back(r);
        }

        return res;
    }
};
```

## 107. 二叉树的层序遍历II

- 链接：[107. 二叉树的层序遍历II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
遍历后重排序；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            vector<int> r;
            int num = q.size();
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();

                r.push_back(cur->val);
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            res.push_back(r);
        }
        reverse(res);
        return res;
    }

    void reverse(vector<vector<int>>& res) {
        int top = 0;
        int end = res.size() - 1;

        while (top < end) {
            vector<int> tmp = res[top];
            res[top] = res[end];
            res[end] = tmp;
            top++;
            end--;
        }
    }
};
```

## 199. 二叉树的右视图

- 链接：[199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
每层的最后一个数据，只记录最后的数据就可以了；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int r;
            int num = q.size();
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();

                r = cur->val;
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            res.push_back(r);
        }

        return res;
    }
};
```

## 637. 二叉树的层平均值

- 链接：[637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
层序遍历每层算平均即可；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            double r = 0;
            int num = q.size();
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();

                r += cur->val;
                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            res.push_back((double)r/num);
        }

        return res;
    }
};
```

## 429. N叉树的层序遍历

- 链接：[429. N叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
把加左右子节点改成遍历children；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
         vector<vector<int>> res;
        if (!root) {
            return res;
        }
        queue<Node*> q;
        q.push(root);

        while (!q.empty()) {
            vector<int> r;
            int num = q.size();
            for (int i = 0; i < num; i++) {
                Node* cur = q.front();
                q.pop();
                r.push_back(cur->val);

                int size = cur->children.size();
                for (int j = 0; j < size; j++) {
                    q.push(cur->children[j]);
                }
            }
            res.push_back(r);
        }

        return res;
    }
};
```

## 515. 在每个树行中找最大值

- 链接：[515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：✅

### 第一想法
每层内比较大小

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<int> res;
        if (!root) {
            return res;
        }
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int r = INT_MIN;
            int num = q.size();
            for (int i = 0; i < num; i++) {
                TreeNode* cur = q.front();
                q.pop();
                r = max(r, cur->val);

                if (cur->left) {
                    q.push(cur->left);
                }
                if (cur->right) {
                    q.push(cur->right);
                }
            }
            res.push_back(r);
        }

        return res;
    }
};
```

## 116. 填充每个节点的下一个右侧节点指针

- 链接：[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：⚠️

### 第一想法
层序遍历，不用队列没想出来；

### 看完题解后的想法
O(1)空间的做法非常巧妙，本层的同一父节点的左右子节点之前必有上一层连好的next，所以可以直接实现本层的移动，只要再在上一层的时候把下一层的不同父节点的相邻右左节点连起来就能实现一层的移动；

充分运用了完美二叉树的性质，非叶子节点必有左右子树；

### 实现中遇到的困难
注意连接每一层的下一个节点不要使用q.empty而是直接使用得到的每层大小进行判断；

### 代码
层序遍历：
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if (!root) {
            return root;
        }
        queue<Node*> q;
        q.push(root);

        while (!q.empty()) {
            int num = q.size();
            for (int i = 0; i < num; i++) {
                Node* cur = q.front();
                q.pop();
                if (i != num - 1) {
                    cur->next = q.front();
                } else {
                    cur->next = nullptr;
                }

                if (cur->left) {
                    q.push(cur->left);
                }
                if (cur->right) {
                    q.push(cur->right);
                }
            }
        }
        return root;
    }
};
```
O(1) 空间做法：
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if (!root) {
            return nullptr;
        }
        Node* leftStart = root;
         // 1. 找到每层的最左节点，并且保证走到叶子节点就停止
        while (leftStart->left) {
            // 2. 从最多节点开始往右看
            Node* cur = leftStart;
            while (cur) {
                // 3. 完美二叉树，非叶子节点必有左右子树，先把自己的左右子树连起来
                cur->left->next = cur->right;
                // 4. 这里的移动是依赖于上一层连起来的next
                if (cur->next) {
                    // 5. 把下一层相邻的不同父节点的右左子树连起来
                    cur->right->next = cur->next->left;
                }
                // 6. cur在本层继续移动
                cur = cur->next;
            }
            // 7. left往下移
            leftStart = leftStart->left;
        }

        return root;
    }
};
```

## 117. 填充每个节点的下一个右侧节点指针II

- 链接：[117. 填充每个节点的下一个右侧节点指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)
- 文章：[代码随想录](https://programmercarl.com/0102.二叉树的层序遍历.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GY4y1u7b2)
- 状态：⚠️

### 第一想法
层序遍历，不用队列没想出来；

### 看完题解后的想法
非常巧妙的思想，116是特殊情况进行了硬编码，本题是更加一般的情况；

简单来说就是给下一层加了一个虚拟头节点，然后只要下一层有子节点，就挂在这个头节点的后面，这样下一层不管是左右齐全，还是只有一个或者没有，都直接全部连在一起；

注意leftStart在移动的时候直接走dummy.next，因为不一定left就有left（这个判断其实就是cur在挂节点到dummy之后的时候判断过了）；

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    Node* connect(Node* root) {
        Node* leftStart = root;
        while (leftStart) {
            // 1. 挂一个虚拟头节点
            Node dummy(0);
            // 2. 挂一个虚拟尾节点
            Node* tail = &dummy;
            Node* cur = leftStart;
            while (cur) {
                // 3. 如果cur有left，就挂在尾节点后面
                if (cur->left) {
                    tail->next = cur->left;
                    tail = tail->next;
                }
                // 3. 如果cur有right，就挂在尾节点后面
                if (cur->right) {
                    tail->next = cur->right;
                    tail = tail->next;
                }
                // 4. 通过上一层挂号的next移动
                cur = cur->next;
            }
            // 5. 因为dummy就是挂在下一层的，所以直接走dummy.next;
            leftStart = dummy.next;
        }
        return root;
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
--

### 实现中遇到的困难
--

### 代码

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
--

### 实现中遇到的困难
--

### 代码

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
深度复习了DFS和BFS，特别是面试喜欢考的O(1)空间复杂度；
