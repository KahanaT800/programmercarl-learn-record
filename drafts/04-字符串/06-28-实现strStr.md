## 28. 实现strStr()

- 链接：[28. 实现strStr()](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
- 文章：[代码随想录](https://programmercarl.com/0028.实现strStr.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1PD4y1o7nd)
- 状态：❌

### 第一想法
KMP算法，但是忘了；

### 看完题解后的想法
KMP算法。详细的 next 数组构建、回退逻辑分析见博客：[KMP 算法详解](/notes/algo/01-kmp算法详解/)

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
