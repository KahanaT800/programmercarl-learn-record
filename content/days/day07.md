---
title: "代码随想录打卡第7天 | 454. 四数相加II、383. 赎金信、15. 三数之和、18. 四数之和"
date: 2026-03-10
tags: [哈希表]
---

## 454. 四数相加II

- 链接：[力扣454](https://leetcode.cn/problems/4sum-ii/)
- 文章：[代码随想录](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Md4y1Q7Yh)
- 状态：⚠️
### 第一想法
two sum的变形，把数组两两分组之后变成two sum；

### 看完题解后的想法
——

### 实现中遇到的困难
这里最开始看到分组都要两层循环，时间复杂度达到 $O(n^2)$ ，就觉得在查询逻辑里可以不用哈希直接遍历，因为也是两层循环；

但是这个是要考虑数组大小的，原来的数组大小是 $n$ ，分组求和之后就是 $n^2$ ，如果再两层遍历循环这个 $n^2$ 的数组复杂度就是 $O(n^4)$ ；

### 代码

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> hash;
        int cnt = 0;

        for (int i = 0; i < nums1.size(); i++) {
            for (int j = 0; j < nums2.size(); j++) {
                hash[nums1[i] + nums2[j]]++;
            }
        }

        for (int i = 0; i < nums3.size(); i++) {
            for (int j = 0; j < nums4.size(); j++) {
                int sum = nums3[i] + nums4[j];
                if (hash.count(-sum) != 0) {
                    cnt += hash[-sum];
                }
            }
        }

        return cnt;
    }
};
```

---

## 383. 赎金信

- 链接：[力扣383](https://leetcode.cn/problems/ransom-note/)
- 文章：[代码随想录](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)
- 状态：✅

### 第一想法
类似于异构词，但是只用部分一致；

### 看完题解后的想法
——

### 实现中遇到的困难
——

### 代码

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int arr[26] = {0};

        for (char c : magazine) {
            arr[c - 'a']++;
        }

        for (char c : ransomNote) {
            if (--arr[c - 'a'] < 0) {
                return false;
            } ;
        }

        return true;
    }
};
```

---

## 15. 三数之和

- 链接：[力扣15](https://leetcode.cn/problems/3sum/)
- 文章：[代码随想录](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1GW4y127qo)
- 状态：⚠️

### 第一想法
第一眼没看出来怎么做；

### 看完题解后的想法
需要先排序，然后定位第一个数，再去移动两个指针，并且三个指针都需要去重；

### 实现中遇到的困难
去重逻辑需要写在找到之后，不能写在外面，不然会漏解：

解的组成可能 l 和 r 指向的数是一样的，如果在找到解的外面去重，就会漏接；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size()-2; i++) {
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            } else {
                int l = i + 1, r = nums.size() - 1;
                while (l < r) {
                    int sum = nums[i] + nums[l] + nums[r];

                    if (sum < 0) {
                        l++;
                    } else if (sum > 0) {
                        r--;
                    } else {
                        result.push_back({nums[i], nums[l], nums[r]});
                        while (l < r && nums[l] == nums[l+1]) { // 走到最后一个重复位置
                            l++;
                        }
                        while (l < r && nums[r] == nums[r-1]) {
                            r--;
                        }
                        l++; // 往前走一格
                        r--;
                    }
                }
            }
        }
        return result;
    }
};

```

---

## 18. 四数之和

- 链接：[力扣18](https://leetcode.cn/problems/4sum/)
- 文章：[代码随想录](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1DS4y147US)
- 状态：✅ 

### 第一想法
在三数和之上加一个坐标的移动；

K数和都是这种方法；

### 看完题解后的想法
——

### 实现中遇到的困难
因为数字很大，需要使用long；

并且直接nums.size()进行判断很容易出错，先转换为int好（因为size返回的是size_t）；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        int n = nums.size();

        for (int i = 0; i < n-3; i++) {
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            } 
            
            for (int j = i + 1; j < n - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                int l = j + 1, r = n - 1;
                while (l < r) {
                    long  sum = (long) nums[i] + nums[j] + nums[l] + nums[r];

                    if (sum < target) {
                        l++;
                    } else if (sum > target) {
                        r--;
                    } else {
                        result.push_back({nums[i], nums[j], nums[l], nums[r]});
                        while (l < r && nums[l] == nums[l+1]) { // 走到最后一个重复位置
                            l++;
                        }
                        while (l < r && nums[r] == nums[r-1]) {
                            r--;
                        }
                        l++; // 往前走一格
                        r--;
                    }
                }
            }
        }
        return result;
    }
};
```

---

## 今日收获
学习了K数和的解题思路；
