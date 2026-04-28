---
title: "代码随想录打卡第53天 | KM106. 岛屿的周长，KM110. 字符串接龙，KM105. 有向图的完全联通"
date: 2026-04-25
tags: [图论, DFS, BFS]
---

## KamaCoder 106. 岛屿的周长

- 链接：[KamaCoder 106. 岛屿的周长](https://kamacoder.com/problempage.php?pid=1178)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0106.岛屿的周长.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV13fjczSEyV)
- 状态：⚠️

### 第一想法
伪装的岛屿题， 单纯的遍历；

注意数学公式 周长 = 4 * 岛屿数量 - 2 * 相邻岛屿数量 （一个岛屿贡献4条边，每有一个相邻岛屿减去两条边）

也可以用每个岛屿与多少个边界和水相邻来算；

### 看完题解后的想法
--

### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    int land = 0, adj = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                land++;
                if (i + 1 < n && grid[i + 1][j] == 1) adj++;
                if (j + 1 < m && grid[i][j + 1] == 1) adj++;
            }
        }
    }

    cout << 4 * land - 2 * adj << endl;
}
```

## KamaCoder 110. 字符串接龙

- 链接：[KamaCoder 110. 字符串接龙](https://kamacoder.com/problempage.php?pid=1183)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0110.字符串接龙.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1QEEizDEC4)
- 状态：❌

### 第一想法
没有思路；

### 看完题解后的想法
隐式图BFS，重点在于如何把这个抽象的字符串变化问题构建成一个图；

BFS找路径一定是最短的，所以这种最短路径第一时间都应想到BFS；

但是和腐烂的橘子这种注重每层都在做什么的不一样，本题层序过程并不关心每层在做什么，所以只需要每个节点加入就可以了；
### 实现中遇到的困难
--

### 代码

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <unordered_set>
#include <queue>

using namespace std;

int main() {
    string beginStr, endStr, str;
    int n;
    cin >> n;
    cin >> beginStr >> endStr;
    unordered_set<string> strSet; // 用set来储存strList，方便查找
    while (n--) {
        cin >> str;
        strSet.insert(str);
    }

    unordered_map<string, int> strMap; // 储存已经访问过的str和到达这个str的path
    strMap.insert(pair<string, int>(beginStr, 1)); // 初始化

    queue<string> strQue;
    strQue.push(beginStr); // 入队

    while (!strQue.empty()) {
        string word = strQue.front();
        strQue.pop();
        int path = strMap[word];

        for (int i = 0; i < word.size(); i++) {
            // 遍历word的每一个位置，进行修改
            string newWord = word;
            for (int j = 0 ; j < 26; j++) {
                newWord[i] = j + 'a';
                if (newWord == endStr) {
                    // 如果newWord找到，则返回当前路径+1（因为最后还要变一步）
                    cout << path + 1 << endl;
                    return 0;
                }

                // 如果newWord在Set中且没被访问过，说明这是层序过程中新找到的一个路径，入队+添加访问信息
                if (strSet.find(newWord) != strSet.end() && strMap.find(newWord) == strMap.end()) {
                    strMap.insert(pair<string, int>(newWord, path + 1));
                    strQue.push(newWord);
                }
            }
        }
    }
    return 0;
}
```

## KamaCoder 105. 有向图的完全联通

- 链接：[KamaCoder 105. 有向图的完全联通](https://kamacoder.com/problempage.php?pid=1177)
- 文章：[代码随想录](https://programmercarl.com/kamacoder/0105.有向图的完全可达性.html)
- 视频：[B站讲解](https://www.bilibili.com/video/BV1QRJ6ziEvJ)
- 状态：✅

### 第一想法
使用层序遍历查找 1 所有可以到达的点；

### 看完题解后的想法


### 实现中遇到的困难
注意层序遍历为了减少访问次数必须在入队的时候就标记（因为可能存在比如 1 有 2 3， 然后 2 和 3 同时指向 4， 如果在出队时才标记 4 会被记录两次）

### 代码
BFS写法：
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>

using namespace std;

int main() {
    int n, k;
    cin >> n >> k;
    vector<vector<int>> grid(n+1);

    while (k--) {
        int s, t;
        cin >> s >> t;
        grid[s].push_back(t);
    }

    vector<bool> visited(n+1, false);
    queue<int> q;
    q.push(1);
    visited[1] = true;
    int count = 1;

    while (!q.empty()) {
        int index = q.front();
        q.pop();

        for (int x : grid[index]) {
            if (!visited[x]) {
                visited[x] = true;
                count++;
                q.push(x);
            }
        }
    }
    int res = count == n ? 1 : -1;
    cout << res << endl;
}
```

DFS后置标记，统一性更好：
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>

using namespace std;

void dfs(vector<vector<int>>& grid, vector<bool>& visited, int key) {
    if (visited[key]) return;
    visited[key] = true;

    for (int x : grid[key]) {
        dfs(grid, visited, x);
    }
}

int main() {
    int n, k;
    cin >> n >> k;
    vector<vector<int>> grid(n+1);

    while (k--) {
        int s, t;
        cin >> s >> t;
        grid[s].push_back(t);
    }

    vector<bool> visited(n+1, false);
    dfs(grid, visited, 1);

    for (int i = 1; i <= n; i++) {
        if (visited[i] == false) {
            cout << -1 << endl;
            return 0;
        }
    }
    cout << 1 << endl;
}
```