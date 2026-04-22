---
title: "代码随想录打卡第50天 | 图论理论基础、深搜理论基础、KM98. 所有可达路径、广搜理论基础"
date: 2026-04-22
tags: [图论, DFS, BFS]
---

## 图论理论基础

先对各个概念有个印象（图的种类、度、连通性、邻接矩阵、邻接表等），后续刷题的过程中每个知识点都会得到巩固。

- 文章：[代码随想录 - 图论理论基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 深搜理论基础

- 文章：[代码随想录 - 图论深搜理论基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E6%B7%B1%E6%90%9C%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## KamaCoder 98. 所有可达路径

- 链接：[KamaCoder 98. 所有可达路径](https://kamacoder.com/problempage.php?pid=1170)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0098.所有可达路径.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1VePeepEpP)
- 状态：⚠️

### 第一想法
简单的遍历回溯，但是 ACM 写法的难点在于输入和输出；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码
邻接矩阵写法：
```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<vector<int>> res;
vector<int> path;
void dfs(vector<vector<int>>& graph, int x, int n) {
    if (x == n) {
        res.push_back(path);
        return;
    }

    for (int i = 1; i <= n; i++) {
      if (graph[x][i] == 1) {
        path.push_back(i);
        dfs(graph, i, n);
        path.pop_back(); // 回溯i
      }
    }
}

int main() {
  int n, m, s, t;
  // 读取节点和边的信息
  cin >> n >> m;
  vector<vector<int>> graph(n+1, vector<int>(n+1, 0)); // 起点是 1
  while (m--) {
    cin >> s >> t;
    // 建立邻接矩阵
    graph[s][t] = 1;
  }

  path.push_back(1); // 因为把回溯放在了每一层，起始节点需要提前放进去
  dfs(graph, 1, n);

  // 输出
  if (res.size() == 0) cout << -1 << endl;
  for (int i = 0; i < res.size(); i++) {
      for (int j = 0; j < res[i].size() - 1; j++) {
          cout << res[i][j] << " ";
      }
      cout << res[i][res[i].size() - 1]  << endl;
  }
}
```

邻接表写法：
```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<vector<int>> res;
vector<int> path;
void dfs(vector<vector<int>>& graph, int x, int n) {
    if (x == n) {
        res.push_back(path);
        return;
    }

    for (int i = 0; i < graph[x].size(); i++) {
        path.push_back(graph[x][i]);
        dfs(graph, graph[x][i], n);
        path.pop_back(); // 回溯
    }
}

int main() {
  int n, m, s, t;
  // 读取节点和边的信息
  cin >> n >> m;
  vector<vector<int>> graph(n+1); // 起点是 1
  while (m--) {
    cin >> s >> t;
    // 建立邻接表
    graph[s].push_back(t);
  }

  path.push_back(1); // 因为把回溯放在了每一层，起始节点需要提前放进去
  dfs(graph, 1, n);

  // 输出
  if (res.size() == 0) cout << -1 << endl;
  for (int i = 0; i < res.size(); i++) {
      for (int j = 0; j < res[i].size() - 1; j++) {
          cout << res[i][j] << " ";
      }
      cout << res[i][res[i].size() - 1]  << endl;
  }
}
```

## 广搜理论基础

- 文章：[代码随想录 - 图论广搜理论基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E5%B9%BF%E6%90%9C%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 今日收获
