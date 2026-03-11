---
title: "代码随想录打卡第8天 | 344. 反转字符串、541. 反转字符串II、54. 替换数字"
date: 2026-03-11
tags: [字符串]
---

## 344. 反转字符串

- 链接：[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)
- 文章：[代码随想录](https://programmercarl.com/0344.反转字符串.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1fV4y17748)
- 状态：✅

### 第一想法
双指针法

### 看完题解后的想法
——

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n = s.size();
        int left = 0, right = n - 1;

        while (left < right) {
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
            left++;
            right--;
        }
    }
};

```

---

## 541. 反转字符串II

- 链接：[541. 反转字符串II](https://leetcode.cn/problems/reverse-string-ii/)
- 文章：[代码随想录](https://programmercarl.com/0541.反转字符串II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1dT411j7NN)
- 状态：✅

### 第一想法
反转字符串写成可传位置的函数，然后一段段翻转，最后处理尾巴；

### 看完题解后的想法
用 for 循环每次跳 2k 更简洁；

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.size();
        int time = n / (2* k);
        int tail = n % (2* k);
        int index = 0;

        while (time > 0) {
            swapStr(s, index, index+k-1);
            index = index + 2 * k ;
            time--;
        }

        if (tail > 0 && tail < k) {
            swapStr(s, index, index+tail-1);
        } else {
            swapStr(s, index, index+k-1);
        }

        return s;
    }

    void swapStr(string& s, int l, int r) {
        int left = l, right = r;

        while (left < right) {
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
            left++;
            right--;
        }
    }
};
```

---

## KamaCoder 54. 替换数字

- 链接：[KamaCoder 54. 替换数字](https://kamacoder.com/problempage.php?pid=1064)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0054.替换数字.html)
- 视频：无
- 状态：⚠️

### 第一想法
刚开始在纠结如何原地扩展；

### 看完题解后的想法
直接拼接一个新的字符串就行，同时注意 `+=` 对字符串进行了重载，字符和字符串都能用；

### 实现中遇到的困难
——

### 代码

```cpp
#include <iostream>
#include <string>
using namespace std;

string addNumber(string& s) {
    string result;
    for (char c : s) {
        if (c >= '0' && c <= '9') {
            result += "number";
        } else {
            result += c;
        }
    }
    return result;
}

int main() {
    string s;
    getline(cin, s);

    cout << addNumber(s) << endl;

    return 0;
}
```

---

## 今日收获

