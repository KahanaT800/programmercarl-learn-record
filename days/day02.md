# 代码随想录打卡第2天 | 209. 长度最小的子数组、59. 螺旋矩阵II、58. 区间和、44. 开发商购买土地

> 日期：2026-03-05
> 学习时长：

## 209. 长度最小的子数组

- 链接：[力扣209](https://leetcode.cn/problems/minimum-size-subarray-sum/)
- 文章：[代码随想录](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1tZ4y1q7XE)
- 状态：✅

### 第一想法
看到这道题的第一想法是做一个前缀和，但是引入前缀和之后需要去减去之后恰好小于target的另一个前缀和，遍历的话复杂度就会变成 $O(n^2)$ ，然后想到了可以二分查找；

但是仔细思考一下就会发现，可以维护一个队列，不断把数据push进去，一旦找到大于target的就开始把前面的数据踢出去，直到再次小于target；这样就不需要复杂的查找了；

### 看完题解后的想法
队列在这里其实就是一个双指针维护的滑动窗口；

### 实现中遇到的困难
result需要判断当前窗口大小和历史result谁更小，使用双指针滑动窗口不会有问题，但是如果是用队列来维护滑动窗口，需要使用queue.size(),返回的类型是size_t，需要强制转换成int；

一开始还给result直接赋值0，但是这样在min里会锁死最小值；

### 代码

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sum = 0;
        int result = INT_MAX;
        int left = 0;

        for (int right = 0; right < nums.size(); right++) {
            sum += nums[right];

            while (sum >= target) {
                result = min(result, right-left+1);

                sum = sum - nums[left];
                left++;
            }
        }

        return result == INT_MAX ? 0 : result;;
    }
};
```

---

## 59. 螺旋矩阵II

- 链接：[力扣59](https://leetcode.cn/problems/spiral-matrix-ii/)
- 文章：[代码随想录](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1SL4y1N7mV)
- 状态：❌

### 第一想法
看完题目的第一想法就是如何移动坐标去填充数据，然后就被边界条件卡住了；

### 看完题解后的想法
比起移动坐标本身，需要移动的是边界，填充不是让坐标自己去找边界，而是让坐标在边界上移动；


### 实现中遇到的困难
沿四条边界进行填充时，需要注意移动方向和限制条件；

### 代码

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n, 0));
        int top = 0, down = n-1;
        int left = 0, right = n-1;
        int target = n * n;
        int num = 1;

        // 填充四个边界，每填充一个边界就把边界向内移动
        while (num <= target) {
            // 1. 填充上边界
            for (int j = left; j <= right; j++) {
                result[top][j] = num;
                num++;
            }
            top++;

            // 2. 填充右边界
            for (int i = top; i <= down; i++) {
                result[i][right] = num;
                num++;
            }
            right--;

            // 3. 填充下边界
            for (int j = right; j >= left; j--) {
                result[down][j] = num;
                num++;
            }
            down--;

            // 4. 填充左边界
            for (int i = down; i >= top; i--) {
                result[i][left] = num;
                num++;
            }
            left++;
        }
        
        return result;
    }
};

```

---

## 58. 区间和

- 链接：[KamaCoder 58](https://kamacoder.com/problempage.php?pid=1070)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html)
- 状态：⚠️

### 第一想法
区间和就是前缀和的差

在数组问题里只要看到连续的某一部分的时候优先考虑前缀和；

### 看完题解后的想法
——

### 实现中遇到的困难
ACM输入输出忘了咋实现了；

注意因为是a到b的区间，所以下标应该是 `b - (a - 1)`;

### 代码

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int n, a, b;
    cin >> n;

    vector<int> input(n);
    vector<int> prevs(n);

    int prev = 0;

    for (int i = 0; i < n; i++) {
        cin >> input[i];
        prev += input[i];
        prevs[i] = prev;
    }

    while (cin >> a >> b) {
        int result = 0;
        if (a == 0) {
            result = prevs[b];
        } else {
            result = prevs[b] - prevs[a-1];
        }
        cout << result << endl;
    }

    return 0;
}
```

---

## 44. 开发商购买土地

- 链接：[KamaCoder 44](https://kamacoder.com/problempage.php?pid=1044)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html)
- 状态：❌

### 第一想法
被分块这个行为卡住了没能理解怎么做；

### 看完题解后的想法
只能横竖分块其实就是计算每行的和与每列的和；然后再按照前缀和的思路来计算块与块之间的差；

最后一项前缀和就是总量，
$$Diff = A - (Total - A) = A - Total + A = 2A - Total$$

### 实现中遇到的困难
——

### 代码

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <climits>
#include <algorithm>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    // 1. 建图
    vector<vector<int>> map(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> map[i][j];
        }
    }

    // 2. 统计每行的前缀和
    vector<int> rows(n, 0);
    int row_sum = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            row_sum += map[i][j];
        }
        rows[i] = row_sum;
    }

    // 3. 统计每列的前缀和
    vector<int> cols(m, 0);
    int col_sum = 0;
    for (int j = 0; j < m; j++) {
        for (int i = 0; i < n; i++) {
            col_sum += map[i][j];
        }
        cols[j] = col_sum;
    }

    // 4. 计算按行分和按列分的最小差值
    int result = INT_MAX;
    for (int i = 0; i < n - 1; i++) {
        result = min(result, abs((rows[n-1] - rows[i])- rows[i]));
    }
    for (int j = 0; j < m - 1; j++) {
        result = min(result, abs((cols[m-1] - cols[j])- cols[j]));
    }

    cout << result << endl;

    return 0;
}
```

---

## 今日收获

深入使用了前缀和，强化了双指针滑动窗口的使用；

螺旋矩阵的让边界自己移动的思路要记住；

