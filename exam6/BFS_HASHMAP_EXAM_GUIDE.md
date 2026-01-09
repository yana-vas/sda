# 🗺️ BFS + HashMap: Пълен Guide за Изпита

## Какво да Очакваш
**"Комбинира хешмап и опашка"** = BFS с `unordered_map` / `unordered_set`

---

# 📌 УНИВЕРСАЛЕН TEMPLATE

```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int result = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        // ====== ЛОГИКА ТУК ======
        result++;
        // ========================
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return result;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    vector<int> results;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            results.push_back(bfs(i));
        }
    }
    
    // Output според задачата
    sort(results.begin(), results.end());
    for (int r : results) {
        cout << r << " ";
    }
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 1: Брой Ребра по Компоненти

## Условие
Даден е ненасочен граф с V върха и E ребра. За всяка свързана компонента изведи броя на ребрата. Изведи резултатите в нарастващ ред.

## Input
```
6 5
0 1
1 2
0 2
3 4
4 5
```

## Output
```
2 3
```
(Компонента 1: върхове 3,4,5 с 2 ребра. Компонента 2: върхове 0,1,2 с 3 ребра)

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int edgeCount = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        edgeCount += graph[node].size();
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return edgeCount / 2;  // Всяко ребро се брои 2 пъти!
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    vector<int> results;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            results.push_back(bfs(i));
        }
    }
    
    sort(results.begin(), results.end());
    for (int r : results) {
        cout << r << " ";
    }
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 2: Брой Върхове по Компоненти

## Условие
Даден е ненасочен граф. За всяка свързана компонента изведи броя на върховете.

## Input
```
7 4
0 1
1 2
3 4
5 6
```

## Output
```
2 2 3
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int nodeCount = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        nodeCount++;  // Просто броим върховете
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return nodeCount;  // БЕЗ деление!
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    // Инициализирай всички върхове (дори изолираните)
    for (int i = 0; i < V; i++) {
        graph[i];  // създава празен vector ако не съществува
    }
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    vector<int> results;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            results.push_back(bfs(i));
        }
    }
    
    sort(results.begin(), results.end());
    for (int r : results) {
        cout << r << " ";
    }
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 3: Брой Свързани Компоненти

## Условие
Даден е ненасочен граф. Колко свързани компоненти има?

## Input
```
7 4
0 1
1 2
3 4
5 6
```

## Output
```
3
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

void bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < V; i++) {
        graph[i];
    }
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    int componentCount = 0;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            bfs(i);
            componentCount++;
        }
    }
    
    cout << componentCount << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 4: Най-кратък Път (без тежести)

## Условие
Даден е ненасочен граф. Намери най-краткия път от връх S до връх T. Ако няма път, изведи -1.

## Input
```
6 7
0 1
0 2
1 3
2 3
3 4
4 5
2 4
0 5
```

## Output
```
3
```
(Път: 0 → 2 → 4 → 5)

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start, int target) {
    queue<pair<int, int>> q;  // {node, distance}
    q.push({start, 0});
    visited.insert(start);
    
    while (!q.empty()) {
        auto [node, dist] = q.front();
        q.pop();
        
        if (node == target) {
            return dist;
        }
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push({adj, dist + 1});
            }
        }
    }
    
    return -1;  // Няма път
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    int S, T;
    cin >> S >> T;
    
    cout << bfs(S, T) << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 5: Има ли Път?

## Условие
Даден е ненасочен граф. Провери дали съществува път между върхове A и B.

## Input
```
5 3
0 1
1 2
3 4
0 4
```

## Output
```
NO
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

bool bfs(int start, int target) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        if (node == target) {
            return true;
        }
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return false;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    int A, B;
    cin >> A >> B;
    
    cout << (bfs(A, B) ? "YES" : "NO") << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 6: Най-голямата Компонента

## Условие
Даден е ненасочен граф. Намери размера (брой върхове) на най-голямата свързана компонента.

## Input
```
8 5
0 1
1 2
2 0
3 4
5 6
```

## Output
```
3
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int size = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        size++;
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return size;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < V; i++) {
        graph[i];
    }
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    int maxSize = 0;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            maxSize = max(maxSize, bfs(i));
        }
    }
    
    cout << maxSize << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 7: Острови в Матрица

## Условие
Дадена е матрица N x M с 0 и 1. Колко "острова" има? (остров = група от свързани 1-ци)

## Input
```
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

## Output
```
3
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
vector<vector<int>> grid;
vector<vector<bool>> visited;

int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

void bfs(int startX, int startY) {
    queue<pair<int, int>> q;
    q.push({startX, startY});
    visited[startX][startY] = true;
    
    while (!q.empty()) {
        auto [x, y] = q.front();
        q.pop();
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx >= 0 && nx < n && ny >= 0 && ny < m &&
                grid[nx][ny] == 1 && !visited[nx][ny]) {
                visited[nx][ny] = true;
                q.push({nx, ny});
            }
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    cin >> n >> m;
    
    grid.resize(n, vector<int>(m));
    visited.resize(n, vector<bool>(m, false));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    
    int islandCount = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1 && !visited[i][j]) {
                bfs(i, j);
                islandCount++;
            }
        }
    }
    
    cout << islandCount << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 8: Размер на Острови

## Условие
Дадена е матрица N x M с 0 и 1. Изведи размерите на всички острови в нарастващ ред.

## Input
```
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

## Output
```
1 2 4
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
vector<vector<int>> grid;
vector<vector<bool>> visited;

int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

int bfs(int startX, int startY) {
    queue<pair<int, int>> q;
    q.push({startX, startY});
    visited[startX][startY] = true;
    
    int size = 0;
    
    while (!q.empty()) {
        auto [x, y] = q.front();
        q.pop();
        
        size++;
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx >= 0 && nx < n && ny >= 0 && ny < m &&
                grid[nx][ny] == 1 && !visited[nx][ny]) {
                visited[nx][ny] = true;
                q.push({nx, ny});
            }
        }
    }
    
    return size;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    cin >> n >> m;
    
    grid.resize(n, vector<int>(m));
    visited.resize(n, vector<bool>(m, false));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    
    vector<int> sizes;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1 && !visited[i][j]) {
                sizes.push_back(bfs(i, j));
            }
        }
    }
    
    sort(sizes.begin(), sizes.end());
    for (int s : sizes) {
        cout << s << " ";
    }
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 9: Заразяване / Разпространение

## Условие
Дадена е матрица N x M. Някои клетки са заразени (2), някои са здрави (1), някои са празни (0). Всяка секунда заразените заразяват съседните здрави. За колко време всичко ще е заразено? Ако е невъзможно, изведи -1.

## Input
```
3 3
2 1 1
1 1 0
0 1 1
```

## Output
```
4
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int n, m;
    cin >> n >> m;
    
    vector<vector<int>> grid(n, vector<int>(m));
    queue<pair<int, int>> q;
    int freshCount = 0;
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
            if (grid[i][j] == 2) {
                q.push({i, j});  // Всички заразени в началото
            } else if (grid[i][j] == 1) {
                freshCount++;
            }
        }
    }
    
    if (freshCount == 0) {
        cout << 0 << endl;
        return 0;
    }
    
    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    
    int time = 0;
    
    while (!q.empty()) {
        int size = q.size();  // Текущо ниво
        
        for (int i = 0; i < size; i++) {
            auto [x, y] = q.front();
            q.pop();
            
            for (int d = 0; d < 4; d++) {
                int nx = x + dx[d];
                int ny = y + dy[d];
                
                if (nx >= 0 && nx < n && ny >= 0 && ny < m &&
                    grid[nx][ny] == 1) {
                    grid[nx][ny] = 2;
                    freshCount--;
                    q.push({nx, ny});
                }
            }
        }
        
        if (!q.empty()) time++;
    }
    
    cout << (freshCount == 0 ? time : -1) << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 10: Приятели до Ниво K

## Условие
Дадена е социална мрежа (граф). От човек S, намери всички хора достижими до K нива разстояние.

## Input
```
6 7
0 1
0 2
1 3
2 3
3 4
4 5
2 4
0
2
```
(6 върха, 7 ребра, старт от 0, K=2)

## Output
```
0 1 2 3 4
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;

vector<int> bfs(int start, int maxLevel) {
    vector<int> result;
    unordered_map<int, int> level;  // node -> level
    
    queue<int> q;
    q.push(start);
    level[start] = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        if (level[node] > maxLevel) continue;
        
        result.push_back(node);
        
        for (int adj : graph[node]) {
            if (!level.count(adj)) {
                level[adj] = level[node] + 1;
                q.push(adj);
            }
        }
    }
    
    return result;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    int S, K;
    cin >> S >> K;
    
    vector<int> reachable = bfs(S, K);
    
    sort(reachable.begin(), reachable.end());
    for (int node : reachable) {
        cout << node << " ";
    }
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 11: Двуделен Граф (Bipartite)

## Условие
Даден е ненасочен граф. Провери дали може да се оцвети с 2 цвята, така че никои два съседни върха да нямат еднакъв цвят.

## Input
```
4 4
0 1
1 2
2 3
3 0
```

## Output
```
YES
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_map<int, int> color;  // -1 = uncolored, 0 or 1 = color

bool bfs(int start) {
    queue<int> q;
    q.push(start);
    color[start] = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        for (int adj : graph[node]) {
            if (!color.count(adj)) {
                color[adj] = 1 - color[node];  // Противоположен цвят
                q.push(adj);
            } else if (color[adj] == color[node]) {
                return false;  // Същият цвят = НЕ е bipartite
            }
        }
    }
    
    return true;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E;
    cin >> V >> E;
    
    for (int i = 0; i < V; i++) {
        graph[i];
    }
    
    for (int i = 0; i < E; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }
    
    bool isBipartite = true;
    for (int i = 0; i < V; i++) {
        if (!color.count(i)) {
            if (!bfs(i)) {
                isBipartite = false;
                break;
            }
        }
    }
    
    cout << (isBipartite ? "YES" : "NO") << endl;
    
    return 0;
}
```

---

# 📝 ЗАДАЧА 12: Най-близък Изход в Лабиринт

## Условие
Дадена е матрица (лабиринт). '.' е път, '#' е стена, 'S' е старт, 'E' е изход. Намери най-краткия път до някой изход. Може да има много изходи.

## Input
```
5 5
#####
#S..#
###.#
#E..#
#####
```

## Output
```
5
```

## Решение
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int n, m;
    cin >> n >> m;
    
    vector<string> grid(n);
    int startX, startY;
    
    for (int i = 0; i < n; i++) {
        cin >> grid[i];
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 'S') {
                startX = i;
                startY = j;
            }
        }
    }
    
    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    
    vector<vector<bool>> visited(n, vector<bool>(m, false));
    queue<tuple<int, int, int>> q;  // {x, y, dist}
    
    q.push({startX, startY, 0});
    visited[startX][startY] = true;
    
    while (!q.empty()) {
        auto [x, y, dist] = q.front();
        q.pop();
        
        if (grid[x][y] == 'E') {
            cout << dist << endl;
            return 0;
        }
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx >= 0 && nx < n && ny >= 0 && ny < m &&
                grid[nx][ny] != '#' && !visited[nx][ny]) {
                visited[nx][ny] = true;
                q.push({nx, ny, dist + 1});
            }
        }
    }
    
    cout << -1 << endl;  // Няма път
    
    return 0;
}
```

---

# 🔑 БЪРЗ СПРАВОЧНИК

## Какво Променяш в BFS

| Задача | Променяш |
|--------|----------|
| Брой ребра | `count += graph[node].size()` + `/2` накрая |
| Брой върхове | `count++` |
| Брой компоненти | Броиш извикванията на BFS |
| Shortest path | `queue<pair<int,int>>` + `dist+1` |
| Има ли път | `return true` когато намериш target |
| Най-голяма компонента | `max(bfs())` |
| По нива | `int size = q.size()` преди вътрешен цикъл |
| Bipartite | `color[adj] = 1 - color[node]` |

## Граф vs Матрица

| Граф | Матрица |
|------|---------|
| `unordered_map<int, vector<int>>` | `vector<vector<int>>` |
| `for (int adj : graph[node])` | `for (int i = 0; i < 4; i++)` с dx/dy |
| `visited.count(adj)` | `visited[nx][ny]` |

---

# ✅ CHECKLIST

- [ ] `visited.insert()` ПРЕДИ `q.push()`
- [ ] Edge count `/2` за ненасочен граф
- [ ] Инициализирай изолирани върхове: `graph[i];`
- [ ] За матрица: проверка за граници `nx >= 0 && ...`
- [ ] `ios_base::sync_with_stdio(false);`

---

**УСПЕХ! 🎯**
