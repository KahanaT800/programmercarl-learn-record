---
title: "代码随想录打卡第6天 | 242. 有效的字母异位词、349. 两个数组的交集、202. 快乐数、1. 两数之和"
date: 2026-03-09
tags: [哈希表，Floyd算法（龟兔赛跑）]
---

## 242. 有效的字母异位词

- 链接：[力扣242](https://leetcode.cn/problems/valid-anagram/)
- 文章：[代码随想录](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1YG411p7BA)
- 状态：✅

### 第一想法
这道题要判断两个字符串包含的字符是否完全一致，第一想法就是哈希表；

仔细思考就会发现因为只有26个字符，所以不需要使用完整的哈希表，只需要使用一个定长数组模拟哈希就行了；

### 看完题解后的想法
——

### 实现中遇到的困难
最开始的时候将 `s` 的写入和判断的 `t` 的减少分开在了两个循环中，其实一次循环就能做；

### 代码

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        
        int cnt[26] = {};
        for (int i = 0; i < s.size(); i++) {
            cnt[s[i] - 'a']++;
            cnt[t[i] - 'a']--;
        }
        
        for (int c : cnt) {
            if (c != 0) {
                return false;
            }
        }
        return true;
    }
};
```

---

## 349. 两个数组的交集

- 链接：[力扣349](https://leetcode.cn/problems/intersection-of-two-arrays/)
- 文章：[代码随想录](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1ba411S7wu)
- 状态：✅

### 第一想法
使用集合去重+记录出现元素,之后再用集合去重;

### 看完题解后的想法
1. 构造集合不需要遍历,直接使用迭代器构造就行;

2. 在遍历第二个数组的过程中找到就删去,直接实现去重;

### 实现中遇到的困难
如何不使用第二个集合实现去重;

### 代码

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set(nums1.begin(), nums1.end());
        vector<int> result;

        for (int n : nums2) {
            if (set.erase(n)) {  // 找到就删掉，既判重又去重
                result.push_back(n);
            }
        }
        return result;
    }
};
```

---

## 202. 快乐数

- 链接：[力扣202](https://leetcode.cn/problems/happy-number/)
- 文章：[代码随想录](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)
- 状态：❌

### 第一想法
没想出来如何判断无限循环；

### 看完题解后的想法
int 范围最大是 $2^{31}-1$，也就是 10 位数。10 位数每位最大是 9，所以平方和上界是 $9^{2} * 10 = 810 $; 

不管 n 多大，只要一步之后，值就掉到 810 以内了。从第二步开始，永远在 1~810 这个范围里打转。如果最后不能收敛到1，说明一定会在某一步中出现一个和之前一样的数字，那么从这个数字开始，之后的每一步就和之前重复了，这就是环；

实际上这是一个判环的问题，除了哈希表之外可以使用Floyd算法判环；
### 实现中遇到的困难
要理解成环需要脑筋转弯一下；

### 代码

```cpp
class Solution {
public:
    int getSum(int n) {
        int sum = 0;

        while (n) {
            int d = n % 10;
            sum = sum + d * d;
            n = n / 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        int slow = n;
        int fast = n;
        do {
            slow = getSum(slow);
            fast = getSum(getSum(fast));
        } while (fast != 1 && slow != fast);

        return fast == 1;
    }
};
```

---

## 1. 两数之和

- 链接：[力扣1](https://leetcode.cn/problems/two-sum/)
- 文章：[代码随想录](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1aT41177mK)
- 状态：✅

### 第一想法
把值作为Key, 下标作为Value;

### 看完题解后的想法
——

### 实现中遇到的困难
先查询， 再插入, 不然会自己查到自己；

### 代码

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;

        for (int i = 0; i < nums.size(); i++) {
            if (map.count(target - nums[i]) != 0) {
                return {i, map[target - nums[i]]};
            }
            map[nums[i]] = i;
        }

        return {};
    }
};
```

---

## 今日收获
对于鸽巢原理和Floyd算法的灵活运用有了新的理解;
