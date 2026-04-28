---
title: "代码随想录打卡第52天 | KM101. 孤岛总面积, KM102 沉没孤岛，KM103 水流问题，KM104 建造最大岛屿"
date: 2026-04-24
tags: [图论, DFS, BFS]
---
## KamaCoder 101. 孤岛的总面积

- 链接：[KamaCoder 101. 孤岛的总面积](https://kamacoder.com/problempage.php?pid=1173)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0101.孤岛的总面积.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1mmZJYRESc)
- 状态：❌

### 第一想法
第一想法是给不是孤岛的岛屿返回标记，但是这样太麻烦了；

### 看完题解后的想法
淹没法，从边界出发，如果不是孤岛直接淹掉，这样最后直接统计面积就可以了；

### 实现中遇到的困难
标记前置法和标记后置法的写法不一样，前置法需要每次都标记；

### 代码
DFS的标记前置写法：
```cpp
#include <iostream>
#include <vector>

using namespace std;
int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

void dfs(vector<vector<int>>& grid, int x, int y) {
    for (int i = 0; i < 4; i++) {
        int nextX = x + dir[i][0];
        int nextY = y + dir[i][1];
        if (nextX < 0 || nextX >= grid.size() || nextY < 0 || nextY >= grid[0].size()) continue;
        if (grid[nextX][nextY] == 1) {
            grid[nextX][nextY] = 0;
            dfs(grid, nextX, nextY);
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

    int res = 0;

    for (int i = 0; i < n; i++) {
        if (grid[i][0] == 1) {
            grid[i][0] = 0;
            dfs(grid, i, 0);
        }
        if (grid[i][m - 1] == 1) {
            grid[i][m - 1] = 0;
            dfs(grid, i, m - 1);
        }
    }

    for (int j = 0; j < m; j++) {
        if (grid[0][j] == 1) {
            grid[0][j] = 0;
            dfs(grid, 0, j);
        }
        if (grid[n - 1][j] == 1) {
            grid[n - 1][j] = 0;
            dfs(grid, n - 1, j);
        }
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                res++;
            }
        }
    }

    cout << res;
}
```
BFS的标记后置写法
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;
int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

void bfs(vector<vector<int>>& grid, int x, int y) {
    if (grid[x][y] != 1) return;
    queue<pair<int, int>> q;
    q.push({x, y});
    grid[x][y] = 0;

    while (!q.empty()) {
        auto cur = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nx = cur.first + dir[i][0];
            int ny = cur.second + dir[i][1];
            if (nx < 0 || nx >= grid.size() || ny < 0 || ny >= grid[0].size()) continue;
            if (grid[nx][ny] != 1) continue;
            grid[nx][ny] = 0;
            q.push({nx, ny});
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

    int res = 0;

    for (int i = 0; i < n; i++) {
        bfs(grid, i, 0);
        bfs(grid, i, m - 1);
    }

    for (int j = 0; j < m; j++) {
        bfs(grid, 0, j);
        bfs(grid, n - 1, j);
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                res++;
            }
        }
    }

    cout << res;
}
```

## KamaCoder 102. 沉没孤岛

- 链接：[KamaCoder 102. 沉没孤岛](https://kamacoder.com/problempage.php?pid=1174)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0102.沉没孤岛.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1fjdWYyEGu)
- 状态：✅

### 第一想法
淹没法，但是是把非孤岛的岛屿给染色，然后在最后遍历的时候将被染色的岛屿还原；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;
int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

void bfs(vector<vector<int>>& grid, int x, int y) {
    if (grid[x][y] != 1) return;
    queue<pair<int, int>> q;
    q.push({x, y});
    grid[x][y] = 2;

    while (!q.empty()) {
        auto cur = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nx = cur.first + dir[i][0];
            int ny = cur.second + dir[i][1];
            if (nx < 0 || nx >= grid.size() || ny < 0 || ny >= grid[0].size()) continue;
            if (grid[nx][ny] != 1) continue;
            grid[nx][ny] = 2;
            q.push({nx, ny});
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

    for (int i = 0; i < n; i++) {
        bfs(grid, i, 0);
        bfs(grid, i, m - 1);
    }

    for (int j = 0; j < m; j++) {
        bfs(grid, 0, j);
        bfs(grid, n - 1, j);
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                grid[i][j] = 0;
            } else if (grid[i][j] == 2) {
                grid[i][j] = 1;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m - 1; j++) {
            cout << grid[i][j] << " ";
        }
        cout << grid[i][m - 1] << endl;
    }
}
```
## KamaCoder 103. 水流问题

- 链接：[KamaCoder 103. 水流问题](https://kamacoder.com/problempage.php?pid=1175)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0103.水流问题.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1WNoEYrEio)
- 状态：❌

### 第一想法
第一个想法是每个点遍历然后DFS看是否能到达边界，做不出来；

### 看完题解后的想法
淹没法的运用，从第一组边界出发和第二组边界出发都能到达的值就是答案；

BFS很适合多源起点，因为边界上每个点都可以作为合法起点，可以一次性全加进去减少重复查找；

### 实现中遇到的困难
BFS多源出发不会写；

### 代码
BFS多源出发:
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}}; // 四个方向
void bfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, queue<pair<int, int>>& q) {
    // 起点的判断由上一层已经决定了，所以可以直接进入循环
    while (!q.empty()) {
        auto cur = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nextx = cur.first + dir[i][0];
            int nexty = cur.second + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
            if (visited[nextx][nexty]) continue;
            if (grid[nextx][nexty] < grid[cur.first][cur.second]) continue;
            // 淹没法，反向流动
            visited[nextx][nexty] = true;
            q.push({nextx, nexty});
        }
    }
}

int main() {
    int m, n;
    cin >> m >> n;
    vector<vector<int>> grid(m, vector<int>(n, 0));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> left(m, vector<bool>(n, false));
    vector<vector<bool>> right(m, vector<bool>(n, false));
    queue<pair<int, int>> leftQue, rightQue;

    for (int i = 0; i < m; i++) {
        left[i][0] = true;
        leftQue.push({i, 0});
        right[i][n - 1] = true;
        rightQue.push({i, n - 1});
    }

    for (int j = 0; j < n; j++) {
        left[0][j] = true;
        leftQue.push({0, j});
        right[m - 1][j] = true;
        rightQue.push({m - 1, j});
    }

    bfs(grid, left, leftQue);
    bfs(grid, right, rightQue);

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // 扫描每一个点，从第一组边界出发和第二组边界出发都能到达的值
            if (left[i][j] && right[i][j]) {
                cout << i << " " << j << endl;
            }
        }
    }
    
}
```
DFS淹没法:
```cpp
#include <iostream>
#include <vector>
using namespace std;

int dir[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}}; // 四个方向
void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    if (visited[x][y]) return;
    visited[x][y] = true;

    for (int i = 0; i < 4; i++) {
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
        if (grid[nextx][nexty] >= grid[x][y]) {
            // 淹没法，反向流动
            dfs(grid, visited, nextx, nexty);
        }
    }
}

int main() {
    int m, n;
    cin >> m >> n;
    vector<vector<int>> grid(m, vector<int>(n, 0));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> left(m, vector<bool>(n, false));
    vector<vector<bool>> right(m, vector<bool>(n, false));

    for (int i = 0; i < m; i++) {
        dfs(grid, left, i, 0); // 左边界
        dfs(grid, right, i, n - 1); // 右边界
    }

    for (int j = 0; j < n; j++) {
        dfs(grid, left, 0, j); // 上边界
        dfs(grid, right, m - 1, j); // 下边界
    }


    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // 扫描每一个点，从第一组边界出发和第二组边界出发都能到达的值
            if (left[i][j] && right[i][j]) {
                cout << i << " " << j << endl;
            }
        }
    }
    
}
```

## KamaCoder 104. 建造最大岛屿

- 链接：[KamaCoder 104. 建造最大岛屿](https://kamacoder.com/problempage.php?pid=1176)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0104.建造最大岛屿.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1Dn5CzZEw1)
- 状态：✅

### 第一想法
第一个想法是遍历每一个0，把这个0当作是1，然后开始扩展算岛屿面积；

### 看完题解后的想法
这道题的优化解比较好理解，就是给岛屿标上不同的记号，然后用哈希表将记号与面积对应起来；

因为暴力解每次从0重新拓展都要反复计算已经出现过的面积，所以优化思想就从如何用以前出现过的信息来解决；

### 实现中遇到的困难
--

### 代码
优化解：标记岛屿，再查表算面积
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>
using namespace std;

int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

int bfs(vector<vector<int>>& grid, int x, int y, int mark) {
    int n = grid.size();
    int m = grid[0].size();
    queue<pair<int, int>> q;
    q.push({x, y});
    // 标记记号
    grid[x][y] = mark;
    int area = 1;
    while (!q.empty()) {
        auto [cx, cy] = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nx = cx + dir[i][0];
            int ny = cy + dir[i][1];
            if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
            if (grid[nx][ny] != 1) continue;
            grid[nx][ny] = mark;
            area++;
            q.push({nx, ny});
        }
    }
    return area;
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    
    // 给每个岛屿建立 编号-面积 对应
    unordered_map<int, int> areaMap;
    int mark = 2;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                areaMap[mark] = bfs(grid, i, j, mark);
                mark++;
            }
        }
    }
    
    // 初始化为最大岛屿面积（有可能全1）
    int res = 0;
    for (auto& [k, v] : areaMap) res = max(res, v);

    // 枚举0， 查表 + 去重
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] != 0) continue;
            
            unordered_set<int> neighbors;
            for (int d = 0; d < 4; d++) {
                int nx = i + dir[d][0];
                int ny = j + dir[d][1];
                if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
                if (grid[nx][ny] == 0) continue;
                neighbors.insert(grid[nx][ny]);
            }
            
            int total = 1;
            for (int mk : neighbors) total += areaMap[mk];
            res = max(res, total);
        }
    }
    
    cout << res;
}
```
暴力解：
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

// 从 (x,y) 出发,把它当作 1,BFS 能连通的所有 1,返回总面积
int bfs(vector<vector<int>>& grid, int x, int y) {
    int n = grid.size();
    int m = grid[0].size();
    vector<vector<bool>> visited(n, vector<bool>(m, false));
    
    queue<pair<int, int>> q;
    q.push({x, y});
    visited[x][y] = true;
    int area = 1;  // (x,y) 自己变成的 1 算进去
    
    while (!q.empty()) {
        auto [cx, cy] = q.front();
        q.pop();
        for (int i = 0; i < 4; i++) {
            int nx = cx + dir[i][0];
            int ny = cy + dir[i][1];
            if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
            if (visited[nx][ny]) continue;
            if (grid[nx][ny] != 1) continue;  // 只扩散到 1
            visited[nx][ny] = true;
            area++;
            q.push({nx, ny});
        }
    }
    return area;
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    
    int res = 0;
    bool hasZero = false;
    
    // 遍历每个 0,假设它变成 1,看能连出多大的岛
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 0) {
                hasZero = true;
                res = max(res, bfs(grid, i, j));
            }
        }
    }
    
    // 特殊情况:全是 1,没有 0 可以变
    if (!hasZero) {
        res = n * m;
    }
    
    cout << res;
    return 0;
}
```