---
title: "代码随想录打卡第10天 | 232. 用栈实现队列、225. 用队列实现栈、20. 有效的括号、1047. 删除字符串中的所有相邻重复项"
date: 2026-03-13
tags: [栈, 队列]
---

## 232. 用栈实现队列

- 链接：[232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)
- 文章：[代码随想录](https://programmercarl.com/0232.用栈实现队列.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1nY4y1w7VC)
- 状态：✅

### 第一想法
一个栈 s1 用来入队，一个栈 s2 用来出队；

### 看完题解后的想法
最开始每次把数据从 s1 倒入 s2 还要把 s2 中的数据倒回去，为了保持顺序一致；

但是其实可以做一个简化，只有s2中空了才从s1中补充，这样其实是分段连续的；

### 实现中遇到的困难
——

### 代码

```cpp
class MyQueue {
public:
    MyQueue() {

    }

    void push(int x) {
        s1.push(x);
    }

    int pop() {
        transfer();
        int res = s2.top();
        s2.pop();

        return res;
    }

    int peek() {
        transfer();
        int res = s2.top();

        return res;
    }

    bool empty() {
        return s1.empty() && s2.empty();
    }
private:
    stack<int> s1;
    stack<int> s2;

    void transfer() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
    }
};
```

---

## 225. 用队列实现栈

- 链接：[225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)
- 文章：[代码随想录](https://programmercarl.com/0225.用队列实现栈.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Fd4y1K7sm)
- 状态：✅

### 第一想法
刚开始用两个queue互相捣腾数据；

### 看完题解后的想法
只需要在每次压入数据的时候，把之前的数据全部取出来再压进去，就变成了栈的样子；

### 实现中遇到的困难
——

### 代码

```cpp
class MyStack {
public:
    MyStack() {

    }

    void push(int x) {
        q.push(x);

        for (int i = 0; i < (int)q.size() - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }

    int pop() {
        int res = q.front();
        q.pop();
        return res;
    }

    int top() {
        return q.front();
    }

    bool empty() {
        return q.empty();
    }
private:
    queue<int> q;
};
```

---

## 20. 有效的括号

- 链接：[20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)
- 文章：[代码随想录](https://programmercarl.com/0020.有效的括号.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1AF411w78g)
- 状态：✅

### 第一想法
第一想法是把左括号都压入stack，然后遇到右括号就进行比较，如果能匹配就弹出左括号；

### 看完题解后的想法
其实可以反过来想，遇到左括号就压入右括号，遇到右括号就比较看看自己在不在栈顶，在的话就弹出说明能匹配，不在直接报错；

### 实现中遇到的困难
按照左括号压入的时候，没有对访问栈顶进行判空处理，会导致bug；

### 代码

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> stack;
        for (char c : s) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            } else {
                if (stack.empty() || c != stack.top()) {
                    return false;
                }
                stack.pop();
            }
         }
        return stack.empty();
    }
};
```

---

## 1047. 删除字符串中的所有相邻重复项

- 链接：[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)
- 文章：[代码随想录](https://programmercarl.com/1047.删除字符串中的所有相邻重复项.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV12a411P7mw)
- 状态：✅

### 第一想法
需要使用栈,但是不能用stack,用string模拟就行了;

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string res;
        for (char c : s) {
            if (!res.empty() && res.back() == c) {
                res.pop_back();
            } else {
                res.push_back(c);
            }
        }
        return res;
    }
};
```

---

## 今日收获

