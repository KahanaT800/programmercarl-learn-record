---
title: "代码随想录打卡第1天 | 704. 二分查找、27. 移除元素、977. 有序数组的平方"
date: 2026-03-04
tags: [数组, 二分查找, 双指针]
---

## 704. 二分查找

- 链接：[力扣704](https://leetcode.cn/problems/binary-search/)
- 文章：[代码随想录](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1fA4y1o715)
- 状态：✅

### 第一想法
二分查找母题；

有闭区间和开区间两种解法：

闭区间：`right` 初始化为 `nums.size()-1` ,是实右边界；在进入  `while` 循环时就要注意需要判断 `left == right`；同时每次移动边界都要从已经找到的地方往右或往左移动一步；

开区间：`right` 初始化为 `nums.size()` ,是虚右边界；在进入  `while` 循环时就要注意写 `left < right`；移动时左边界是 `mid+1`， 而右边界是 `mid`；

### 看完题解后的想法
——

### 实现中遇到的困难
采用实变界进入循环的判断忘记了需要判断`left == right`；

### 代码

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;

        while (left <= right) {
            int mid = left + (right-left)/2;

            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                right = mid-1;
            } else {
                left = mid+1;
            }
        }

        return -1;
    }
};

```

---

## 27. 移除元素

- 链接：[力扣27](https://leetcode.cn/problems/remove-element/)
- 文章：[代码随想录](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV12A4y1Z7LP)
- 状态：⚠️

### 第一想法
第一想法就是维护两个指针，`left` 找到 `val` 位置，而 `right` 从右边找不为val的值，当左右相遇说明找完了；

### 看完题解后的想法
——

### 实现中遇到的困难
最开始`right = nums.size()-1`就导致了需要额外判断刚开始左右重合时的情况；

并且因为需要左右重叠去找，计数也有点困难；

这里可以让右边的移动不用那么快，而是一步步移动，即使是 `val`，`left` 被赋值之后也不会动，直到 `right` 找到不为val的值；

并且为了简化初始的判断，让 `right = nums.size()` ；这样right的位置就自带了计数功能；

### 代码

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0;
        int right = nums.size();

        while (left < right) {
            if (nums[left] == val) {
                nums[left] = nums[right-1];
                right--;
            } else {
                left++;
            }
        }

        return right;
    }
};

```

---

## 977. 有序数组的平方

- 链接：[力扣977](https://leetcode.cn/problems/squares-of-a-sorted-array/)
- 文章：[代码随想录](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1QB4y1D7ep)
- 状态：✅

### 第一想法
有序，说明最大值可能出现在两端；

要按照升序排列平方数的话，就需要双指针比较，哪个大哪个放在后面然后再移动放了数据的指针；

### 看完题解后的想法
——

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int left = 0;
        int right = nums.size()-1;
        vector<int> res(nums.size(), 0);

        for (int i = nums.size()-1; i >=  0; i--) {
            if (nums[left] * nums[left] > nums[right] * nums[right]) {
                res[i] = nums[left] * nums[left];
                left++;
            } else {
                res[i] = nums[right] * nums[right];
                right--;
            }
        }

        return res;
    }
};

```

---

## 今日收获

复习了二分查找的开区间和闭区间；

踩坑了双指针法的边界条件；

