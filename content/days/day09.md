---
title: "代码随想录打卡第9天 | 151. 翻转字符串里的单词、55. 右旋字符串、28. 实现strStr()、459. 重复的子字符串"
date: 2026-03-12
tags: [字符串, KMP]
---

## 151. 翻转字符串里的单词

- 链接：[151. 翻转字符串里的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)
- 文章：[代码随想录](https://programmercarl.com/0151.翻转字符串里的单词.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1uT41177fX)
- 状态：⚠️

### 第一想法
第一想法感觉这道题好麻烦，然后拆解任务一共三步：

1. 去掉两端以及中见多余的空格
2. 翻转整个字符串
3. 挨着翻转每个单词

其中在挨着翻转每个单词的时候因为尾端没有空格，所以临时加上一个空格在翻转完之后再去掉，避免了尾处理；
### 看完题解后的想法
原地去空格使用双指针法， 最后需要resize，注意使用的size就是slow最后停留的index；

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    string reverseWords(string s) {
        endSpace(s);
        reverse(s, 0, (int)s.size()-1);

        int left = 0;
        int right = 0;
        while (right <= (int)s.size()) {
            if (right == (int)s.size() || s[right] == ' ') {
                reverse(s, left, right-1);
                left = ++right;
            } else {
                right++;
            }
        }

        return s;
    }

    // 去掉两端以及中见多余的空格
    void endSpace(string& s) {
        int slow = 0;
        for (int fast = 0; fast < (int)s.size(); fast++) {
            if (s[fast] != ' ') {
                if (slow > 0) {
                    s[slow++] = ' ';
                }
                while (fast < (int)s.size() && s[fast] != ' ') {
                    s[slow++] = s[fast++];
                }
            }
        }
        s.resize(slow);
    }

    // 翻转函数
    void reverse(string& s, int left, int right) {
        int l = left;
        int r = right;

        while (l < r) {
            char tmp = s[r];
            s[r] = s[l];
            s[l] = tmp;
            l++;
            r--;
        }
    }
};
```

---

## KamaCoder 55. 右旋字符串

- 链接：[KamaCoder 55. 右旋字符串](https://kamacoder.com/problempage.php?pid=1065)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0055.右旋字符串.html)
- 视频：无
- 状态：⚠️

### 第一想法
刚开始是准备整体移动+栈来构造新的字符串；

### 看完题解后的想法
其实就是简单的三次翻转；

### 实现中遇到的困难
输入之后需要使用cin.ignore();忽略换行符；

### 代码

```cpp
#include <iostream>
#include <string>

using namespace std;

void reverse(string& s, int left, int right) {
    int l = left;
    int r = right;

    while (l < r) {
        char tmp = s[r];
        s[r] = s[l];
        s[l] = tmp;
        l++;
        r--;
    }
}

int main() {
    int n;
    string s;
    cin >> n;
    cin.ignore();
    getline(cin, s);

    n %= s.size();
    reverse(s, 0, (int)s.size()-1);
    reverse(s, 0, n-1);
    reverse(s, n, (int)s.size()-1);

    cout << s << endl;
    return 0;
}
```

---

## 28. 实现strStr()

- 链接：[28. 实现strStr()](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
- 文章：[代码随想录](https://programmercarl.com/0028.实现strStr.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1PD4y1o7nd)
- 状态：❌

### 第一想法
KMP算法，但是忘了；

### 看完题解后的想法
KMP算法。详细的 next 数组构建、回退逻辑分析见博客：[KMP 算法详解]({{< ref "/notes/algo/02-KMP算法详解" >}})

核心要点：
- `next[i]` = `pattern[0..i]` 的最长相等前后缀长度
- 失配时通过 `j = next[j-1]` 回退，用次长前缀在当前位置再试
- 构建 next 和匹配的逻辑完全一样

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) {
            return 0;
        }

        vector<int> next(m, 0);
        int j = 0;
        for (int i = 1; i < m; i++) {
            while (j > 0 && needle[i] != needle[j]) {
                j = next[j - 1];
            }
            if (needle[i] == needle[j]) {
                j++;
            }
            next[i] = j;
        }

        j = 0;
        for (int i = 0; i < n; i++) {
            while (j > 0 && haystack[i] != needle[j]) {
                j = next[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
     }
};
```
---

## 459. 重复的子字符串

- 链接：[459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)
- 文章：[代码随想录](https://programmercarl.com/0459.重复的子字符串.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1cg41127fw)
- 状态：❌

### 第一想法
第一想法是从0开始加长子串，然后每个字串用KMP遍历；

这么做需要维护匹配次数；

### 看完题解后的想法
假设 $s$ 由子串 $t$ 重复 $k$ 次构成 $(k >= 2)$，即 $s = t * k$ 。

那么 $s + s = t * 2k$ 。

去掉首尾各一个字符后，原本位置 $0$ 和位置 $len(s)$ 的匹配被破坏了。但 $s$ 还可以从位置 $len(t)$ 开始匹配，因为 $s + s$ 从位置 $len(t)$ 开始就是 $t * (2k-1)$ ，长度足够容纳一个完整的 $s = t * k$ 。而 $len(t) >= 1$ 且 $len(t) <= len(s)/2$ ，所以这个匹配位置在去掉首尾字符之后仍然存在。

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string doubled= s + s;
        string text = doubled.substr(1, (int)doubled.size()-2);
        return KMP(text, s);
    }

    bool KMP(string& text, string& pattern) {
        int n = text.size(), m = pattern.size();

        vector<int> next(m, 0);
        int j = 0;
        for (int i = 1; i < m; i++) {
            while (j > 0 && pattern[i] != pattern[j]) {
                j = next[j - 1];
            }
            if (pattern[i] == pattern[j]) {
                j++;
            }
            next[i] = j;
        }

        j = 0;
        for (int i = 0; i < n; i++) {
            while (j > 0 && text[i] != pattern[j]) {
                j = next[j - 1];
            }
            if (text[i] == pattern[j]) {
                j++;
            }
            if (j == m) {
                return true;
            }
        }
        return false;
    }
};
```

---

## 今日收获

字符串的翻转操作很灵活——翻转单词本质是「整体翻转 + 局部翻转」，右旋字符串也是三次翻转。

KMP 是字符串匹配的核心算法，关键在于理解 next 数组的构建和回退逻辑。

重复子串特别巧妙地运用了数学，值得多看几遍；
