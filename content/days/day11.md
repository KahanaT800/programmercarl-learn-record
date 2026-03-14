---
title: "代码随想录打卡第11天 | 150. 逆波兰表达式求值、239. 滑动窗口最大值、347. 前K个高频元素"
date: 2026-03-14
tags: [栈, 队列]
---

## 150. 逆波兰表达式求值

- 链接：[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)
- 文章：[代码随想录](https://programmercarl.com/0150.逆波兰表达式求值.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1kd4y1o7on)
- 状态：⚠️

### 第一想法
逆波兰表达式

逆波兰表达式：

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) * ( 3 + 4 ) 。

该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) * ) 。

逆波兰表达式主要有以下两个优点：

去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + * 也可以依据次序计算出正确结果。

适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中

### 看完题解后的想法
——

### 实现中遇到的困难
注意第一次出栈的是右操作数，第二次出栈的是左操作数；

### 代码

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;

        for (string& t : tokens) {
            if (t == "+" || t == "-" || t == "*" || t == "/") {
                int a = st.top();
                st.pop();
                int b = st.top();
                st.pop();
                if (t == "+") {
                    st.push(b + a);
                } else if (t == "-") {
                    st.push(b - a);
                } else if (t == "*") {
                    st.push(b * a);
                } else {
                    st.push(b / a);
                }
            } else {
                st.push(stoll(t));
            }
        }

        return st.top();
    }
};
```

---

## 239. 滑动窗口最大值

- 链接：[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)
- 文章：[代码随想录](https://programmercarl.com/0239.滑动窗口最大值.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1XS4y1p7qj)
- 状态：⚠️

### 第一想法
老员工比新员工菜就优化，老员工大于35也优化

裁员广进算法

### 看完题解后的想法
太恶毒了

### 实现中遇到的困难
最开始在滑动窗口形成上用了双指针，但是其实可以考虑从滑动窗口形成（当前坐标至少能放下一个滑动窗口）才开始采集；

### 代码

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq;
        vector<int> res;

        for (int i = 0; i < (int)nums.size(); i++) {
            // 老员工大于"35"优化
            if (!dq.empty() && dq.front() <= i - k) {
                dq.pop_front();
            }

            // 老员工比新员工菜优化
            while (!dq.empty() && nums[dq.back()] < nums[i]) {
                dq.pop_back();
            }

            // 新员工入队
            dq.push_back(i);

            // 窗口形成后开始采集
            if (i >= k - 1) {
                res.push_back(nums[dq.front()]);
            }
        }

        return res;
    }
};
```

---

## 347. 前K个高频元素

- 链接：[347. 前K个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)
- 文章：[代码随想录](https://programmercarl.com/0347.前K个高频元素.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Xg41167Lz)
- 状态：❌

### 第一想法
第一反应是用优先队列（堆）来存 频率-值 pair；

### 看完题解后的想法
桶排序，把值放进 n+1 个以频率区分的桶中， 优点是重复频率的值也能放进同一个桶；

### 实现中遇到的困难
注意桶里可以能包含多个值，在统计个数时，应放在桶级，而不是遍历桶的时候；

### 代码
桶排序
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        for (int n : nums) {
            hash[n]++;
        }

        vector<vector<int>> bucket((int)nums.size() + 1);
        for (auto& [key, val] : hash) {
            bucket[val].push_back(key);
        }

        vector<int> res;
        for (int i = (int)bucket.size() - 1; i >= 0 && (int)res.size() < k; i--) {
            for (int n : bucket[i]) {
                res.push_back(n);
                if (res.size() == k) {
                    break;
                }
            }
        }

        return res;
    }
};
```

堆排序
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        for (int n : nums) {
            hash[n]++;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>,  greater<pair<int,int>>> minHeap;
        for (auto& [key, val] : hash) {
            minHeap.push({val, key});
            if ((int)minHeap.size() > k) {
                minHeap.pop();
            }
        }

        vector<int> res;
        while (!minHeap.empty()) {
            res.push_back(minHeap.top().second);
            minHeap.pop();
        }

        return res;
    }
};
```

---

## 今日收获

