# C++ Exam Guide: Dijkstra & Hash Maps
## Пълно ръководство за 6-ка

---

## 🎯 КАКВО ДА ОЧАКВАШ НА ИЗПИТА

> **Задача 1**: Dijkstra = обхождане + нещо мъничко допълнително
> **Задача 2**: Целия материал досега (най-вероятно hash maps)

---

## 📋 Съдържание
1. [Бързи Темплейти](#бързи-темплейти)
2. [Dijkstra - Пълно Обяснение](#dijkstra---пълно-обяснение)
3. [Вариации на Dijkstra](#вариации-на-dijkstra)
4. [Grid Dijkstra Patterns](#grid-dijkstra-patterns)
5. [0-1 BFS](#0-1-bfs)
6. [Hash Maps - Втора Задача](#hash-maps---втора-задача)
7. [Важни C++ Функции](#важни-c-функции)
8. [Как да Четем Условия](#как-да-четем-условия)
9. [Типични Задачи](#типични-задачи)
10. [Времево Разпределение](#времево-разпределение)
11. [Чести Грешки](#чести-грешки)

---

## Бързи Темплейти

### Стандартен Header (винаги започвай с това)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    // твоят код тук
    
    return 0;
}
```

### Базов Dijkstra Template (Function-Based)
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, int> pli;
const ll INF = 1e18;

int V;
vector<vector<pair<int, ll>>> graph;

vector<ll> dijkstra(int start) {
    vector<ll> dist(V, INF);
    dist[start] = 0;
    
    priority_queue<pli, vector<pli>, greater<pli>> pq;
    pq.push({0, start});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto [v, w] : graph[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    
    return dist;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int E;
    cin >> V >> E;
    
    graph.resize(V);
    
    for (int i = 0; i < E; i++) {
        int u, v;
        ll w;
        cin >> u >> v >> w;
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});  // за ненасочен граф
    }
    
    vector<ll> dist = dijkstra(0);
    
    cout << (dist[V-1] == INF ? -1 : dist[V-1]) << endl;
    
    return 0;
}
```

### Dijkstra с Custom Struct (за Grid)
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int x, y;
    int cost;
    
    bool operator<(const Node& other) const {
        return cost > other.cost;  // MIN heap!
    }
};

const int dx[] = {0, 0, 1, -1};
const int dy[] = {1, -1, 0, 0};

int dijkstraGrid(vector<vector<int>>& grid, int startX, int startY) {
    int m = grid.size();
    int n = grid[0].size();
    
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    dist[startX][startY] = 0;
    
    priority_queue<Node> pq;
    pq.push({startX, startY, 0});
    
    while (!pq.empty()) {
        auto [x, y, cost] = pq.top();
        pq.pop();
        
        if (cost > dist[x][y]) continue;
        if (x == m-1 && y == n-1) return cost;  // early exit
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            
            int newCost = cost + grid[nx][ny];  // модифицирай тук!
            
            if (newCost < dist[nx][ny]) {
                dist[nx][ny] = newCost;
                pq.push({nx, ny, newCost});
            }
        }
    }
    
    return dist[m-1][n-1];
}
```

---

## Dijkstra - Пълно Обяснение

### Какво е Dijkstra?
Greedy алгоритъм за намиране на най-кратък път от един начален връх до всички останали в **претеглен граф с НЕотрицателни тежести**.

### Сложност
| Имплементация | Време | Памет |
|---------------|-------|-------|
| Priority Queue (Binary Heap) | O(E log E) | O(V + E) |
| С обикновен масив | O(V²) | O(V) |

**Винаги използвай Priority Queue** за изпит - по-бързо е за sparse graphs.

### Ключови Принципи

1. **Релаксация**: Ако намерим по-кратък път до връх, обновяваме дистанцията
   ```cpp
   if (dist[u] + weight < dist[v]) {
       dist[v] = dist[u] + weight;
   }
   ```

2. **Greedy избор**: Винаги обработваме върха с най-малка текуща дистанция

3. **Visited check**: Пропускаме връх ако `d > dist[u]` (вече сме го обработили с по-добра дистанция)

### Защо работи?
Когато извадим връх от priority queue с минимална дистанция, сме сигурни че това е най-краткият път до него (защото всички тежести са ≥ 0).

---

## Вариации на Dijkstra

### 1. Dijkstra с Изчакване на Интервали (Задача Банско)

**Ключова модификация**: Когато стигнем до връх, трябва да изчакаме следващия автобус.

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, int> pli;
const ll INF = 1e18;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E, start, end_node;
    cin >> V >> E >> start >> end_node;
    
    vector<int> interval(V);
    for (int i = 0; i < V; i++) {
        cin >> interval[i];
    }
    
    vector<vector<pair<int, ll>>> graph(V);
    for (int i = 0; i < E; i++) {
        int u, v;
        ll w;
        cin >> u >> v >> w;
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});
    }
    
    vector<ll> dist(V, INF);
    dist[start] = 0;
    
    priority_queue<pli, vector<pli>, greater<pli>> pq;
    pq.push({0, start});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto [v, w] : graph[u]) {
            // Изчисляваме време за изчакване
            ll current_time = dist[u];
            ll wait_time = 0;
            
            if (interval[u] > 0) {
                ll remainder = current_time % interval[u];
                if (remainder != 0) {
                    wait_time = interval[u] - remainder;
                }
            }
            
            ll arrival_time = current_time + wait_time + w;
            
            if (arrival_time < dist[v]) {
                dist[v] = arrival_time;
                pq.push({dist[v], v});
            }
        }
    }
    
    cout << (dist[end_node] == INF ? -1 : dist[end_node]) << endl;
    
    return 0;
}
```

**Формула за изчакване**:
```cpp
// Ако сме в момент T и автобусът идва на всеки I минути
// Следващият автобус е в момент: ceil(T / I) * I
// Изчакване = ceil(T / I) * I - T

// Или по-просто:
ll wait = (interval > 0 && current_time % interval != 0) 
          ? interval - (current_time % interval) 
          : 0;
```

### 2. Dijkstra с Ограничение (Binary Search + Dijkstra)

**Задача Дядо Коледа**: Намери минималните килограми за сваляне, така че да има път ≤ max_time.

**Подход**: Binary search по отговора + проверка с Dijkstra

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, int> pli;
const ll INF = 1e18;

struct Edge {
    int to;
    ll weight_loss;  // килограми за сваляне
    ll time;
};

int V;
vector<vector<Edge>> graph;

// Проверява дали можем да стигнем за max_time
// използвайки само тунели с weight_loss <= max_weight
bool canReach(int start, int end_node, ll max_weight, ll max_time) {
    vector<ll> dist(V, INF);
    dist[start] = 0;
    
    priority_queue<pli, vector<pli>, greater<pli>> pq;
    pq.push({0, start});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto& e : graph[u]) {
            // Пропускаме тунели, които изискват > max_weight
            if (e.weight_loss > max_weight) continue;
            
            if (dist[u] + e.time < dist[e.to]) {
                dist[e.to] = dist[u] + e.time;
                pq.push({dist[e.to], e.to});
            }
        }
    }
    
    return dist[end_node] <= max_time;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int E;
    ll max_time;
    cin >> V >> E >> max_time;
    
    graph.resize(V);
    ll max_possible_weight = 0;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        ll w, t;
        cin >> u >> v >> w >> t;
        u--; v--;  // 0-indexed
        graph[u].push_back({v, w, t});
        max_possible_weight = max(max_possible_weight, w);
    }
    
    int start = 0, end_node = V - 1;
    
    // Binary search по килограмите
    ll lo = 0, hi = max_possible_weight, ans = -1;
    
    while (lo <= hi) {
        ll mid = (lo + hi) / 2;
        
        if (canReach(start, end_node, mid, max_time)) {
            ans = mid;
            hi = mid - 1;  // търсим по-малко
        } else {
            lo = mid + 1;
        }
    }
    
    cout << ans << endl;
    
    return 0;
}
```

### 3. Dijkstra в Матрица (Grid)

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef pair<int, pair<int, int>> piii;  // {dist, {row, col}}
const int INF = 1e9;
const int dx[] = {0, 0, 1, -1};
const int dy[] = {1, -1, 0, 0};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int N, M;
    cin >> N >> M;
    
    vector<string> grid(N);
    for (int i = 0; i < N; i++) {
        cin >> grid[i];
    }
    
    vector<vector<int>> dist(N, vector<int>(M, INF));
    dist[0][0] = 0;
    
    priority_queue<piii, vector<piii>, greater<piii>> pq;
    pq.push({0, {0, 0}});
    
    while (!pq.empty()) {
        auto [d, pos] = pq.top();
        auto [x, y] = pos;
        pq.pop();
        
        if (d > dist[x][y]) continue;
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= N || ny < 0 || ny >= M) continue;
            
            int cost = (grid[nx][ny] == '#') ? 1 : 0;
            
            if (dist[x][y] + cost < dist[nx][ny]) {
                dist[nx][ny] = dist[x][y] + cost;
                pq.push({dist[nx][ny], {nx, ny}});
            }
        }
    }
    
    cout << dist[N-1][M-1] << endl;
    
    return 0;
}
```

---

## Grid Dijkstra Patterns

### Pattern 1: Custom Struct с operator<
```cpp
struct Node {
    int x, y;
    int cost;
    
    // За MIN heap - обърни знака!
    bool operator<(const Node& other) const {
        return cost > other.cost;  // ВАЖНО: > за min heap
    }
};

// Използване:
priority_queue<Node> pq;
pq.push({0, 0, 0});
```

### Pattern 2: Grid Setup
```cpp
int m = grid.size();
int n = grid[0].size();
vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

bool isValid(int x, int y, int m, int n) {
    return x >= 0 && x < m && y >= 0 && y < n;
}
```

### Pattern 3: Minimum Effort Path (MAX вместо SUM)
**Ключова разлика**: `newCost = max(current.cost, edgeCost)` вместо `+`

```cpp
struct Node {
    int x, y, effort;
    bool operator<(const Node& other) const {
        return effort > other.effort;
    }
};

int minimumEffortPath(vector<vector<int>>& heights) {
    int m = heights.size(), n = heights[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    priority_queue<Node> pq;
    
    pq.push({0, 0, 0});
    dist[0][0] = 0;
    
    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    
    while (!pq.empty()) {
        auto [x, y, effort] = pq.top();
        pq.pop();
        
        if (effort > dist[x][y]) continue;
        if (x == m-1 && y == n-1) return effort;
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            
            // MAX вместо SUM!
            int newEffort = max(effort, abs(heights[nx][ny] - heights[x][y]));
            
            if (newEffort < dist[nx][ny]) {
                dist[nx][ny] = newEffort;
                pq.push({nx, ny, newEffort});
            }
        }
    }
    return 0;
}
```

### Pattern 4: Max Probability (MAX heap + умножение)
**Ключова разлика**: MAX heap, умножение вместо събиране

```cpp
double maxProbability(int n, vector<vector<int>>& edges, 
                      vector<double>& prob, int start, int end) {
    vector<vector<pair<int, double>>> graph(n);
    for (int i = 0; i < edges.size(); i++) {
        graph[edges[i][0]].push_back({edges[i][1], prob[i]});
        graph[edges[i][1]].push_back({edges[i][0], prob[i]});
    }
    
    vector<double> dist(n, 0);  // 0, не INF!
    dist[start] = 1;  // 1, не 0!
    
    // MAX heap - без greater<>!
    priority_queue<pair<double, int>> pq;
    pq.push({1, start});
    
    while (!pq.empty()) {
        auto [prob, u] = pq.top();
        pq.pop();
        
        if (prob < dist[u]) continue;  // < вместо >
        if (u == end) return prob;
        
        for (auto [v, w] : graph[u]) {
            double newProb = prob * w;  // * вместо +
            if (newProb > dist[v]) {    // > вместо <
                dist[v] = newProb;
                pq.push({newProb, v});
            }
        }
    }
    return 0;
}
```

### Pattern 5: Grid с Directions/Arrows (0-1 cost)
```cpp
// directions: 1=right, 2=left, 3=down, 4=up
vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

int minCost(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    
    deque<tuple<int, int, int>> dq;  // {x, y, cost}
    dq.push_front({0, 0, 0});
    dist[0][0] = 0;
    
    while (!dq.empty()) {
        auto [x, y, cost] = dq.front();
        dq.pop_front();
        
        if (cost > dist[x][y]) continue;
        if (x == m-1 && y == n-1) return cost;
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dirs[i][0];
            int ny = y + dirs[i][1];
            
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            
            // Cost = 0 ако следваме стрелката, 1 иначе
            int addCost = (grid[x][y] == i + 1) ? 0 : 1;
            int newCost = cost + addCost;
            
            if (newCost < dist[nx][ny]) {
                dist[nx][ny] = newCost;
                if (addCost == 0) dq.push_front({nx, ny, newCost});
                else dq.push_back({nx, ny, newCost});
            }
        }
    }
    return -1;
}
```

### Pattern 6: Cheapest Flights with K Stops
```cpp
int findCheapestPrice(int n, vector<vector<int>>& flights, 
                      int src, int dst, int k) {
    vector<vector<pair<int, int>>> adj(n);
    for (auto& e : flights) {
        adj[e[0]].push_back({e[1], e[2]});
    }
    
    // stops[i] = минимален брой спирки за да стигнем до i
    vector<int> stops(n, INT_MAX);
    
    // {cost, node, numStops}
    priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> pq;
    pq.push({0, src, 0});
    
    while (!pq.empty()) {
        auto curr = pq.top();
        pq.pop();
        
        int cost = curr[0];
        int node = curr[1];
        int numStops = curr[2];
        
        // Skip ако вече имаме по-добър път с по-малко спирки
        if (numStops >= stops[node] || numStops > k + 1) continue;
        stops[node] = numStops;
        
        if (node == dst) return cost;
        
        for (auto [next, price] : adj[node]) {
            pq.push({cost + price, next, numStops + 1});
        }
    }
    return -1;
}
```

### Pattern 7: Multiple Edges - Keep Minimum
```cpp
// Ако има повече от един път между два върха
unordered_map<int, unordered_map<int, int>> graph;

for (int i = 0; i < E; i++) {
    cin >> u >> v >> w;
    
    // Запази само най-евтиния edge
    if (!graph[u].count(v) || graph[u][v] > w) {
        graph[u][v] = w;
        graph[v][u] = w;
    }
}
```

### Pattern 8: Grid Encoding (2D → 1D)
```cpp
// Encode (row, col) -> single int
int encode(int row, int col, int cols) {
    return row * cols + col;
}

// Decode back
pair<int, int> decode(int pos, int cols) {
    return {pos / cols, pos % cols};
}

// Използване с visited
vector<bool> visited(rows * cols, false);
int pos = encode(x, y, cols);
visited[pos] = true;
```

---

## 0-1 BFS

**Кога да използваме**: Когато тежестите са само 0 или 1.

**По-бързо от Dijkstra**: O(V + E) вместо O(E log E)

**Ключова идея**: Използваме `deque` вместо priority queue:
- Ребро с тежест 0 → добавяме отпред
- Ребро с тежест 1 → добавяме отзад

```cpp
#include <bits/stdc++.h>
using namespace std;

const int INF = 1e9;
const int dx[] = {0, 0, 1, -1};
const int dy[] = {1, -1, 0, 0};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int N, M;
    cin >> N >> M;
    
    vector<string> grid(N);
    for (int i = 0; i < N; i++) {
        cin >> grid[i];
    }
    
    vector<vector<int>> dist(N, vector<int>(M, INF));
    
    deque<pair<int, int>> dq;
    
    // Намираме всички starting points (примерно от края на матрицата)
    // За задачата с Ели - всички клетки на ръба
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            if (i == 0 || i == N-1 || j == 0 || j == M-1) {
                int cost = (grid[i][j] == '#') ? 1 : 0;
                dist[i][j] = cost;
                if (cost == 0) {
                    dq.push_front({i, j});
                } else {
                    dq.push_back({i, j});
                }
            }
        }
    }
    
    while (!dq.empty()) {
        auto [x, y] = dq.front();
        dq.pop_front();
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= N || ny < 0 || ny >= M) continue;
            
            int cost = (grid[nx][ny] == '#') ? 1 : 0;
            
            if (dist[x][y] + cost < dist[nx][ny]) {
                dist[nx][ny] = dist[x][y] + cost;
                
                if (cost == 0) {
                    dq.push_front({nx, ny});
                } else {
                    dq.push_back({nx, ny});
                }
            }
        }
    }
    
    // Намираме максималната дистанция сред коридорите
    int max_dist = 0;
    int count = 0;
    
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            if (grid[i][j] == '.' && dist[i][j] != INF) {
                if (dist[i][j] > max_dist) {
                    max_dist = dist[i][j];
                    count = 1;
                } else if (dist[i][j] == max_dist) {
                    count++;
                }
            }
        }
    }
    
    cout << count << endl;
    
    return 0;
}
```

---

## Hash Maps - Втора Задача

> **Очаквай**: BFS/DFS + hash map, counting, или graph с hash map

### Основни Операции
```cpp
#include <unordered_map>
#include <unordered_set>

unordered_map<int, int> mp;
mp[key] = value;           // insert/update
mp.count(key);             // 0 или 1
mp.erase(key);             // изтрий
mp.find(key) != mp.end();  // проверка

unordered_set<int> st;
st.insert(x);
st.count(x);
st.erase(x);
```

### Pattern 1: Graph с Hash Map (от контролното)
```cpp
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
    return edgeCount / 2;  // всяко ребро се брои 2 пъти
}
```

### Pattern 2: K-th Smallest с Max Heap (от контролното)
```cpp
int k;
cin >> k;

priority_queue<int> maxHeap;  // max heap за k най-малки

int num;
while (cin >> num && num != -1) {
    if (num == 0) {
        cout << (maxHeap.size() < k ? -1 : maxHeap.top()) << endl;
        continue;
    }
    
    maxHeap.push(num);
    if (maxHeap.size() > k) {
        maxHeap.pop();  // премахни най-големия
    }
}
```

### Pattern 3: Counting с Hash Map
```cpp
unordered_map<int, int> freq;

for (int x : arr) {
    freq[x]++;
}

// Намери елемент с честота > n/2
for (auto& [val, count] : freq) {
    if (count > n / 2) return val;
}
```

### Pattern 4: Two Sum Pattern
```cpp
unordered_map<int, int> seen;  // value -> index

for (int i = 0; i < n; i++) {
    int complement = target - arr[i];
    if (seen.count(complement)) {
        return {seen[complement], i};
    }
    seen[arr[i]] = i;
}
```

### Pattern 5: Connected Components Count
```cpp
unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int countComponents(int V) {
    int count = 0;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            bfs(i);  // или dfs(i)
            count++;
        }
    }
    return count;
}
```

### Pattern 6: Group Anagrams / Grouping
```cpp
unordered_map<string, vector<string>> groups;

for (string& s : strs) {
    string key = s;
    sort(key.begin(), key.end());
    groups[key].push_back(s);
}
```

### Pattern 7: Sliding Window + Hash Map
```cpp
unordered_map<char, int> window;
int left = 0;

for (int right = 0; right < s.size(); right++) {
    window[s[right]]++;
    
    while (/* условие за невалиден прозорец */) {
        window[s[left]]--;
        if (window[s[left]] == 0) {
            window.erase(s[left]);
        }
        left++;
    }
    
    // update answer
}
```

### Pattern 8: Graph Adjacency с unordered_map
```cpp
// За графи където върховете не са 0..n-1
unordered_map<int, unordered_map<int, int>> graph;  // graph[u][v] = weight

// Добавяне на ребро
graph[u][v] = w;
graph[v][u] = w;

// Итерация през съседи
for (auto& [neighbor, weight] : graph[node]) {
    // ...
}
```

### Кога да използваш Hash Map vs Vector?

| Ситуация | Използвай |
|----------|-----------|
| Върхове от 0 до n-1 | `vector<vector<int>>` |
| Върхове с произволни ID-та | `unordered_map` |
| Броене на честоти | `unordered_map<T, int>` |
| Проверка за съществуване | `unordered_set` |
| Visited за graph | `unordered_set<int>` или `vector<bool>` |

---

## Важни C++ Функции

### Priority Queue

```cpp
// MIN heap (за Dijkstra - ВИНАГИ използвай това)
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> minHeap;

// MAX heap (default)
priority_queue<int> maxHeap;

// Операции
pq.push({dist, node});
pq.top();      // връща най-малкия/големия
pq.pop();      // премахва
pq.empty();    // проверка
pq.size();     // размер
```

### Vector

```cpp
vector<int> v(n);           // n елемента, default value
vector<int> v(n, INF);      // n елемента с INF
vector<vector<int>> v(n, vector<int>(m, 0));  // 2D

v.push_back(x);
v.pop_back();
v.size();
v.empty();
v.clear();
v[i];
v.front();
v.back();
```

### Unordered Map / Set (Hash)

```cpp
unordered_map<int, int> mp;
mp[key] = value;
mp.count(key);   // 0 или 1
mp.erase(key);

unordered_set<int> st;
st.insert(x);
st.count(x);
st.erase(x);
```

### Useful Math

```cpp
// Ceiling division
ll ceil_div(ll a, ll b) {
    return (a + b - 1) / b;
}

// Следващото кратно на interval >= current
ll next_multiple(ll current, ll interval) {
    if (interval == 0) return current;
    return ceil_div(current, interval) * interval;
}

// Изчакване до следващия автобус
ll wait_time(ll current, ll interval) {
    if (interval == 0) return 0;
    ll remainder = current % interval;
    return (remainder == 0) ? 0 : interval - remainder;
}
```

### Structured Bindings (C++17)

```cpp
// Вместо:
pair<int, int> p = pq.top();
int d = p.first;
int u = p.second;

// Пиши:
auto [d, u] = pq.top();

// За nested pairs:
auto [d, pos] = pq.top();
auto [x, y] = pos;
```

---

## Как да Четем Условия

### Checkpoint 1: Какъв е графът?

| Ключова дума | Тип |
|--------------|-----|
| "насочен/ориентиран/еднопосочен" | Directed |
| "ненасочен" или нищо не пише | Undirected |
| "матрица/grid" | Grid graph |
| "тежест/разстояние/време/цена" | Weighted |

### Checkpoint 2: Какво търсим?

| Условие | Алгоритъм |
|---------|-----------|
| "най-кратък път" + положителни тежести | Dijkstra |
| "най-кратък път" + 0/1 тежести | 0-1 BFS |
| "най-кратък път" + отрицателни тежести | Bellman-Ford |
| "минимално X за да има път с условие" | Binary Search + Dijkstra |
| "колко най-много/най-малко" клетки | BFS/Dijkstra + анализ |

### Checkpoint 3: Има ли допълнителни условия?

- **Интервали/изчакване** → Модифицирай релаксацията
- **Две ограничения** → Binary search по едното
- **От всички точки до край** → Reverse graph или multi-source

---

## Типични Задачи

### ЗАДАЧА 1: Dijkstra Вариации

| Тип | Разпознаване | Модификация |
|-----|--------------|-------------|
| Standard | "Най-кратък път" | Basic template |
| Wait time | "интервал", "чакане", "автобус" | `wait = (interval - time % interval) % interval` |
| Grid | Матрица, координати | 2D dist, dx/dy arrays |
| 0-1 BFS | Cost е 0 или 1 | `deque` + push_front/back |
| Max path | "минимално усилие", "minimum effort" | `max()` вместо `+` |
| Max probability | "вероятност", "probability" | MAX heap + `*` вместо `+` |
| K stops | "най-много K спирки" | Track stops в state |
| Binary search | "минимално X за път с Y" | BS по X + Dijkstra check |

### ЗАДАЧА 2: Hash Map / Graph Вариации

| Тип | Разпознаване | Подход |
|-----|--------------|--------|
| Component counting | "свързани компоненти", "групи" | BFS/DFS + visited set |
| Edge counting | "брой ребра в компонент" | `edgeCount / 2` |
| K-th element | "k-тия най-малък/голям" | Heap с size k |
| Frequency | "колко пъти", "най-често" | `unordered_map<T, int>` |
| Graph с ID-та | Върхове не са 0..n-1 | `unordered_map` за graph |

---

## Времево Разпределение

### За 45 минути (1 задача):

| Време | Действие |
|-------|----------|
| 0-5 мин | Прочети внимателно. Определи типа задача. |
| 5-10 мин | Напиши template + input четене |
| 10-30 мин | Имплементирай алгоритъма |
| 30-40 мин | Тествай с примерите. Debug. |
| 40-45 мин | Edge cases. Финални проверки. |

### За 90 минути (2 задачи):

| Време | Действие |
|-------|----------|
| 0-5 мин | Прочети двете задачи. Започни с по-лесната. |
| 5-40 мин | Първа задача (пълно решение) |
| 40-45 мин | Пауза. Прочети втората отново. |
| 45-85 мин | Втора задача |
| 85-90 мин | Final review на двете |

### Golden Rules:
1. **Не губи време в оптимизации в началото** - първо напиши работещо решение
2. **Тествай с примерите преди submit**
3. **Ако не минава - проверни overflow (използвай `long long`)**
4. **Ако TLE - провери дали имаш излишни операции в цикъла**

---

## Чести Грешки

### 1. Integer Overflow
```cpp
// ❌ ГРЕШНО
int dist[N];
dist[i] + weight  // може да overflow-не

// ✅ ПРАВИЛНО
long long dist[N];
// или
typedef long long ll;
```

### 2. Забравен `greater<>` за min-heap
```cpp
// ❌ ГРЕШНО - това е MAX heap!
priority_queue<pair<int,int>> pq;

// ✅ ПРАВИЛНО - MIN heap
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
```

### 3. Липсващ skip за вече обработени върхове
```cpp
// ❌ ГРЕШНО - без skip
while (!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();
    for (auto [v, w] : graph[u]) { ... }
}

// ✅ ПРАВИЛНО - със skip
while (!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();
    if (d > dist[u]) continue;  // ВАЖНО!
    for (auto [v, w] : graph[u]) { ... }
}
```

### 4. Грешна индексация (0 vs 1)
```cpp
// Провери условието - върховете от 0 или от 1?
// Ако от 1:
u--; v--;  // конвертирай към 0-indexed
```

### 5. Ненасочен граф - само един edge
```cpp
// ❌ ГРЕШНО за ненасочен граф
graph[u].push_back({v, w});

// ✅ ПРАВИЛНО - добави и в двете посоки
graph[u].push_back({v, w});
graph[v].push_back({u, w});
```

### 6. Използване на INF който е твърде малък
```cpp
// ❌ ГРЕШНО - може да не е достатъчно
const int INF = 1e6;

// ✅ ПРАВИЛНО
const long long INF = 1e18;
```

### 7. Забравен ios_base::sync_with_stdio(false)
```cpp
// ❌ БАВНО
int main() {
    int n; cin >> n;
}

// ✅ БЪРЗО
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int n; cin >> n;
}
```

---

## Решение на Примерните Задачи

### Задача 1: Банско (Bus Intervals)

**Анализ**:
- Стандартен Dijkstra с модификация
- При всяко тръгване от спирка, изчакваме следващия автобус
- Формула: `wait = (interval - current_time % interval) % interval`

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, int> pli;
const ll INF = 1e18;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int V, E, S, T;
    cin >> V >> E >> S >> T;
    
    vector<ll> interval(V);
    for (int i = 0; i < V; i++) {
        cin >> interval[i];
    }
    
    vector<vector<pair<int, ll>>> graph(V);
    for (int i = 0; i < E; i++) {
        int u, v;
        ll w;
        cin >> u >> v >> w;
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});
    }
    
    vector<ll> dist(V, INF);
    dist[S] = 0;
    
    priority_queue<pli, vector<pli>, greater<pli>> pq;
    pq.push({0, S});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto [v, w] : graph[u]) {
            ll current = dist[u];
            ll wait = 0;
            
            if (interval[u] > 0 && current % interval[u] != 0) {
                wait = interval[u] - (current % interval[u]);
            }
            
            ll arrival = current + wait + w;
            
            if (arrival < dist[v]) {
                dist[v] = arrival;
                pq.push({dist[v], v});
            }
        }
    }
    
    cout << (dist[T] == INF ? -1 : dist[T]) << endl;
    
    return 0;
}
```

### Задача 2: Ели (0-1 BFS от края)

**Анализ**:
- Multi-source 0-1 BFS от всички крайни клетки
- Коридор = 0, стена = 1
- Намираме max дистанция сред коридорите

```cpp
#include <bits/stdc++.h>
using namespace std;

const int INF = 1e9;
const int dx[] = {0, 0, 1, -1};
const int dy[] = {1, -1, 0, 0};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int N, M;
    cin >> N >> M;
    
    vector<string> grid(N);
    for (int i = 0; i < N; i++) {
        cin >> grid[i];
    }
    
    vector<vector<int>> dist(N, vector<int>(M, INF));
    deque<pair<int, int>> dq;
    
    // Multi-source: всички крайни клетки
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            if (i == 0 || i == N-1 || j == 0 || j == M-1) {
                int cost = (grid[i][j] == '#') ? 1 : 0;
                dist[i][j] = cost;
                if (cost == 0) dq.push_front({i, j});
                else dq.push_back({i, j});
            }
        }
    }
    
    while (!dq.empty()) {
        auto [x, y] = dq.front();
        dq.pop_front();
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= N || ny < 0 || ny >= M) continue;
            
            int cost = (grid[nx][ny] == '#') ? 1 : 0;
            
            if (dist[x][y] + cost < dist[nx][ny]) {
                dist[nx][ny] = dist[x][y] + cost;
                if (cost == 0) dq.push_front({nx, ny});
                else dq.push_back({nx, ny});
            }
        }
    }
    
    int max_dist = -1;
    int count = 0;
    
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            if (grid[i][j] == '.') {
                if (dist[i][j] > max_dist) {
                    max_dist = dist[i][j];
                    count = 1;
                } else if (dist[i][j] == max_dist) {
                    count++;
                }
            }
        }
    }
    
    cout << count << endl;
    
    return 0;
}
```

### Задача 3: Дядо Коледа (Binary Search + Dijkstra)

**Анализ**:
- Две ограничения: килограми и време
- Binary search по килограмите
- За всяко X: проверяваме с Dijkstra дали има път ≤ max_time използвайки само тунели с kg ≤ X

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, int> pli;
const ll INF = 1e18;

struct Edge {
    int to;
    ll kg, time;
};

int V;
vector<vector<Edge>> graph;

bool canReach(ll max_kg, ll max_time) {
    vector<ll> dist(V, INF);
    dist[0] = 0;
    
    priority_queue<pli, vector<pli>, greater<pli>> pq;
    pq.push({0, 0});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto& e : graph[u]) {
            if (e.kg > max_kg) continue;
            
            if (dist[u] + e.time < dist[e.to]) {
                dist[e.to] = dist[u] + e.time;
                pq.push({dist[e.to], e.to});
            }
        }
    }
    
    return dist[V-1] <= max_time;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int E;
    ll max_time;
    cin >> V >> E >> max_time;
    
    graph.resize(V);
    ll max_kg = 0;
    
    for (int i = 0; i < E; i++) {
        int u, v;
        ll kg, t;
        cin >> u >> v >> kg >> t;
        u--; v--;
        graph[u].push_back({v, kg, t});
        max_kg = max(max_kg, kg);
    }
    
    ll lo = 0, hi = max_kg, ans = -1;
    
    while (lo <= hi) {
        ll mid = (lo + hi) / 2;
        
        if (canReach(mid, max_time)) {
            ans = mid;
            hi = mid - 1;
        } else {
            lo = mid + 1;
        }
    }
    
    cout << ans << endl;
    
    return 0;
}
```

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                    DIJKSTRA CHEAT SHEET                     │
├─────────────────────────────────────────────────────────────┤
│ HEADER:                                                     │
│   #include <bits/stdc++.h>                                  │
│   ios_base::sync_with_stdio(false); cin.tie(nullptr);       │
├─────────────────────────────────────────────────────────────┤
│ MIN HEAP:                                                   │
│   priority_queue<pii, vector<pii>, greater<pii>> pq;        │
│   ИЛИ struct с operator< { return a > b; }                  │
├─────────────────────────────────────────────────────────────┤
│ MAX HEAP (за max probability):                              │
│   priority_queue<pii> pq;  // без greater<>                 │
├─────────────────────────────────────────────────────────────┤
│ INIT:                                                       │
│   vector<ll> dist(V, INF);                                  │
│   dist[start] = 0;                                          │
│   pq.push({0, start});                                      │
├─────────────────────────────────────────────────────────────┤
│ LOOP:                                                       │
│   while (!pq.empty()) {                                     │
│       auto [d, u] = pq.top(); pq.pop();                     │
│       if (d > dist[u]) continue;  // ВАЖНО!                 │
│       for (auto [v, w] : graph[u]) {                        │
│           if (dist[u] + w < dist[v]) {                      │
│               dist[v] = dist[u] + w;                        │
│               pq.push({dist[v], v});                        │
│           }                                                 │
│       }                                                     │
│   }                                                         │
├─────────────────────────────────────────────────────────────┤
│ ВАРИАЦИИ НА РЕЛАКСАЦИЯ:                                     │
│   Standard:  dist[u] + w < dist[v]  →  dist[u] + w          │
│   Max path:  max(dist[u], w) < dist[v]  →  max(dist[u], w)  │
│   Max prob:  dist[u] * w > dist[v]  →  dist[u] * w          │
├─────────────────────────────────────────────────────────────┤
│ 0-1 BFS: deque + push_front(0) / push_back(1)               │
├─────────────────────────────────────────────────────────────┤
│ GRID: dx[]={0,0,1,-1}, dy[]={1,-1,0,0}                      │
│       isValid: x>=0 && x<m && y>=0 && y<n                   │
├─────────────────────────────────────────────────────────────┤
│ WAIT TIME: (interval - current % interval) % interval       │
├─────────────────────────────────────────────────────────────┤
│ HASH MAP:                                                   │
│   unordered_map<int, int> mp;  mp[k]=v; mp.count(k);        │
│   unordered_set<int> st;  st.insert(x); st.count(x);        │
└─────────────────────────────────────────────────────────────┘
```

---

## Обобщение: Какво да Очакваш

### Задача 1: Dijkstra + нещо малко
**Най-вероятни вариации:**
- Grid Dijkstra (матрица)
- Wait time / intervals
- Max вместо sum
- 0-1 BFS
- K stops ограничение

### Задача 2: Hash Maps + Graph
**Най-вероятни типове:**
- BFS/DFS с hash map graph
- Counting components
- K-th element с heap
- Frequency counting

---

**Успех на изпита! 🎯**
