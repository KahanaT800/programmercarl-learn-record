---
title: "代码随想录打卡第51天 | KM99. 岛屿数量"
date: 2026-04-23
tags: [图论, DFS, BFS]
---

## KamaCoder 99. 岛屿数量

- 链接：[KamaCoder 99. 岛屿数量](https://kamacoder.com/problempage.php?pid=1171)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0099.岛屿的数量深搜.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV18PRGYcEiB)
- 状态：❌

### 第一想法
第一想法是往右往下找岛屿，但是卡住了；

### 看完题解后的想法
我的想法有点类似于DP，是在遍历过程中找答案，但是DP取决于局部状态，而岛屿的连通性是一个全局属性，

在 (i,j) 做的决策,可能被后面扫到的格子(更下、更右)推翻；

而DFS的思想是先确定这个点是不是岛屿，如果是就把相邻的岛屿全部标记起来；

然后遍历直接全量遍历，只是跳过已经标记过的岛屿；

### 实现中遇到的困难
--

### 代码
DFS：
```cpp
#include <iostream>
#include <vector>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}}; // 四个方向
void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    for (int i = 0; i < 4; i++) {
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        // 越界则跳过
        if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
        if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
            visited[nextx][nexty] = true;
            dfs(grid, visited, nextx, nexty);
        }
    }
}

int main() {
    int m , n;
    cin >> m >> n;
    vector<vector<int>> grid(m, vector<int>(n, 0));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(m, vector<bool>(n, false));
    int res = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // 扫描每一个点，如果该点为岛屿且没被访问过则+1
            if (!visited[i][j] && grid[i][j] == 1) {
                res++;
                visited[i][j] = true;
                // 将与该岛屿相连的所有岛屿标记
                dfs(grid, visited, i, j);
            }
        }
    }

    cout << res;
}
```

BFS：
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}}; // 四个方向
void bfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> q;
    q.push({x, y});
    visited[x][y] = true;
    while (!q.empty()) {
        auto cur = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
                visited[nextx][nexty] = true;
                q.push({nextx, nexty});
            }
        }
    }
}

int main() {
    int m , n;
    cin >> m >> n;
    vector<vector<int>> grid(m, vector<int>(n, 0));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(m, vector<bool>(n, false));
    int res = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // 扫描每一个点，如果该点为岛屿且没被访问过则+1
            if (!visited[i][j] && grid[i][j] == 1) {
                res++;
                // 将与该岛屿相连的所有岛屿标记
                bfs(grid, visited, i, j);
            }
        }
    }

    cout << res;
}
```

## KamaCoder 100. 岛屿的最大面积

- 链接：[KamaCoder 100. 岛屿的最大面积](https://kamacoder.com/problempage.php?pid=1172)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0100.岛屿的最大面积.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1FzoyY5EXH)
- 状态：✅

### 第一想法
统计面积就是在每次找到岛屿的时候开始计数；

### 看完题解后的想法


### 实现中遇到的困难


### 代码
DFS写法
```cpp
#include <iostream>
#include <vector>

using namespace std;
int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y, int& s) {
    for (int i = 0; i < 4; i++) {
        int nextX = x + dir[i][0];
        int nextY = y + dir[i][1];
        if (nextX < 0 || nextX >= grid.size() || nextY < 0 || nextY >= grid[0].size()) continue;
        if (!visited[nextX][nextY] && grid[nextX][nextY] == 1) {
            s++;
            visited[nextX][nextY] = true;
            dfs(grid, visited, nextX, nextY, s);
        }
    }
}

int main() {
    int n, m, s, t;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n, vector<bool>(m, false));
    int res = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int s = 0;
            if (!visited[i][j] && grid[i][j] == 1) {
                s++;
                visited[i][j] = true;
                dfs(grid, visited, i, j, s);
            }
            res = max(res, s);
        }
    }

    cout << res;
}
```
BFS写法
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;
int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

void bfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y, int& s) {
    queue<pair<int, int>> q;
    q.push({x, y});
    visited[x][y] = true;
    while (!q.empty()) {
        auto cur = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
                s++;
                visited[nextx][nexty] = true;
                q.push({nextx, nexty});
            }
        }
    }
}

int main() {
    int n, m, s, t;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n, vector<bool>(m, false));
    int res = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int s = 0;
            if (!visited[i][j] && grid[i][j] == 1) {
                s++;
                visited[i][j] = true;
                bfs(grid, visited, i, j, s);
            }
            res = max(res, s);
        }
    }

    cout << res;
}
```