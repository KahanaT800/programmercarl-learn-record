---
title: "代码随想录打卡第55天 | 并查集，KM 107. 寻找存在的路径"
date: 2026-04-27
tags: [图论, 并查集]
---

## KamaCoder 107. 寻找存在的路径

- 链接：[KamaCoder 107. 寻找存在的路径](https://kamacoder.com/problempage.php?pid=1179)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0107.寻找存在的路径.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1k3T7zWEVj)
- 状态：⚠️

### 第一想法
并查集的题，并查集总体上是个模板，如何把图变成并查集是一个需要熟练的过程

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> father;
// 初始化
void init(int n) {
    father.resize(n);
    for (int i = 0; i < n; i++) {
        father[i] = i;
    }
}
// 路径压缩 + 查找根
int find(int x) {
    if (father[x] != x) father[x] = find(father[x]);
    return father[x];
}
// 合并两个集合
void join(int v, int u) {
    int rootV = find(v);
    int rootU = find(u);
    if (rootV == rootU) return;
    father[rootU] = rootV;
}
// 判断是否在一个集合
bool isSame(int v, int u) {
    int rootV = find(v);
    int rootU = find(u);
    return rootV == rootU;
}

int main() {
    int n, m, s, t, source, destination;
    cin >> n >> m;
    init(n + 1);
    while (m--) {
        cin >> s >> t;
        // 将每个元素加入并查集
        join(s, t);
    }
    cin >> source >> destination;
    // 只要source和destination在一个集合，无向图就能到达
    int res = isSame(source, destination) ? 1 : 0;
    cout << res;
}
```