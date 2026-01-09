# 🎯 C++ Изпит: Пълен Guide за 6-ка

## КАКВО ДА ОЧАКВАШ

| Задача | Описание | Време |
|--------|----------|-------|
| **Задача 1** | Стандартна Dijkstra + една сметка | ~40-45 мин |
| **Задача 2** | Hash map + опашка (BFS) | ~40-45 мин |

---

# 📌 ЗАДАЧА 1: DIJKSTRA

## Универсален Template (НАУЧИ НАИЗУСТ)

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
        
        for (auto& [v, w] : graph[u]) {
            // ====== СМЕТКАТА Е ТУК ======
            ll newDist = dist[u] + w;
            // ============================
            
            if (newDist < dist[v]) {
                dist[v] = newDist;
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
        u--; v--;  // ако е 1-indexed
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});  // ако е ненасочен
    }
    
    vector<ll> dist = dijkstra(0);
    
    cout << (dist[V-1] == INF ? -1 : dist[V-1]) << endl;
    
    return 0;
}
```

---

## Grid Dijkstra Template

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll INF = 1e18;

int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int m, n;
    cin >> m >> n;
    
    vector<vector<int>> grid(m, vector<int>(n));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> grid[i][j];
        }
    }
    
    vector<vector<ll>> dist(m, vector<ll>(n, INF));
    dist[0][0] = 0;
    
    // {dist, x, y}
    priority_queue<tuple<ll, int, int>,
                   vector<tuple<ll, int, int>>,
                   greater<tuple<ll, int, int>>> pq;
    pq.push({0, 0, 0});
    
    while (!pq.empty()) {
        auto [d, x, y] = pq.top();
        pq.pop();
        
        if (d > dist[x][y]) continue;
        if (x == m-1 && y == n-1) {
            cout << d << endl;
            return 0;
        }
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            
            // ====== СМЕТКАТА Е ТУК ======
            ll newDist = d + grid[nx][ny];
            // ============================
            
            if (newDist < dist[nx][ny]) {
                dist[nx][ny] = newDist;
                pq.push({newDist, nx, ny});
            }
        }
    }
    
    cout << (dist[m-1][n-1] == INF ? -1 : dist[m-1][n-1]) << endl;
    
    return 0;
}
```

---

## ВЪЗМОЖНИ "СМЕТКИ" ЗА ИЗПИТА

### 1. Wait Time (Банско стил) ⭐ НАЙ-ВЕРОЯТНО
```cpp
// Чакаш автобус/влак на интервал
ll wait = (interval[u] - dist[u] % interval[u]) % interval[u];
ll newDist = dist[u] + wait + w;
```

**Разпознаване**: "интервал", "автобус тръгва на всеки X минути", "чакане"

### 2. Max вместо Sum
```cpp
// Минимизираш максималното тегло по пътя
ll newDist = max(dist[u], w);
```

**Разпознаване**: "минимално усилие", "minimum effort", "най-трудното ребро"

### 3. Такса/Penalty на връх
```cpp
// Плащаш такса на всеки връх
ll newDist = dist[u] + w + tax[v];
```

**Разпознаване**: "такса", "penalty", "цена на спирка"

### 4. Отваряне в определено време
```cpp
// Можеш да влезеш само след определено време
ll arrival = dist[u] + w;
ll newDist = max(arrival, openTime[v]);
```

**Разпознаване**: "отваря в", "достъпен от време"

### 5. Network Delay (max накрая)
```cpp
// Стандартна Dijkstra, после max
vector<ll> dist = dijkstra(start);

ll ans = 0;
for (int i = 0; i < V; i++) {
    if (dist[i] == INF) {
        cout << -1 << endl;
        return 0;
    }
    ans = max(ans, dist[i]);
}
cout << ans << endl;
```

**Разпознаване**: "сигнал до всички", "време да стигне до всички"

### 6. Counting по време на обхождане
```cpp
// Брой нещо докато обхождаш
int count = 0;

while (!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();
    
    if (d > dist[u]) continue;
    
    if (someCondition[u]) count++;  // БРОЕНЕ
    
    // ... останалата логика
}
```

**Разпознаване**: "колко върха", "брой", "използвани ребра"

---

## ПРИМЕРНИ ЗАДАЧИ ЗА DIJKSTRA

### Пример 1: Банско (Wait Time)
**Условие**: Автобуси тръгват на интервали. Намери най-бързия път.

```cpp
for (auto& [v, w] : graph[u]) {
    ll wait = 0;
    if (interval[u] > 0 && dist[u] % interval[u] != 0) {
        wait = interval[u] - (dist[u] % interval[u]);
    }
    ll newDist = dist[u] + wait + w;
    
    if (newDist < dist[v]) {
        dist[v] = newDist;
        pq.push({dist[v], v});
    }
}
```

### Пример 2: Network Delay Time
**Условие**: От връх K, колко време да стигне сигнал до всички?

```cpp
vector<ll> dist = dijkstra(k);

ll ans = 0;
for (int i = 1; i <= n; i++) {
    if (dist[i] == INF) {
        cout << -1 << endl;
        return 0;
    }
    ans = max(ans, dist[i]);
}
cout << ans << endl;
```

### Пример 3: Minimum Effort Path
**Условие**: Минимизирай максималната разлика по пътя.

```cpp
for (int i = 0; i < 4; i++) {
    int nx = x + dx[i];
    int ny = y + dy[i];
    
    if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
    
    // MAX вместо SUM!
    ll effort = abs(grid[nx][ny] - grid[x][y]);
    ll newDist = max(d, effort);
    
    if (newDist < dist[nx][ny]) {
        dist[nx][ny] = newDist;
        pq.push({newDist, nx, ny});
    }
}
```

### Пример 4: Grid с Obstacles
**Условие**: 0 = празно, 1 = obstacle. Минимум obstacles за премахване.

```cpp
for (int i = 0; i < 4; i++) {
    int nx = x + dx[i];
    int ny = y + dy[i];
    
    if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
    
    ll newDist = d + grid[nx][ny];  // 0 или 1
    
    if (newDist < dist[nx][ny]) {
        dist[nx][ny] = newDist;
        pq.push({newDist, nx, ny});
    }
}
```

### Пример 5: Влакове (Counting rails)
**Условие**: Колко влакови линии можем да премахнем?

```cpp
// PQ: {dist, isRail, node} - сортира се лексикографски
priority_queue<tuple<ll, int, int>,
               vector<tuple<ll, int, int>>,
               greater<tuple<ll, int, int>>> pq;

int usedRails = 0;

while (!pq.empty()) {
    auto [d, rail, u] = pq.top();
    pq.pop();
    
    if (d > dist[u]) continue;
    
    if (rail) usedRails++;  // COUNTING
    
    for (auto& [v, w, isRail] : graph[u]) {
        if (d + w < dist[v]) {
            dist[v] = d + w;
            pq.push({dist[v], isRail, v});
        }
    }
}

cout << k - usedRails << endl;
```

---

# 📌 ЗАДАЧА 2: HASH MAP + ОПАШКА (BFS)

## Универсален Template (НАУЧИ НАИЗУСТ)

```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<int, vector<int>> graph;
unordered_set<int> visited;

int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int result = 0;  // каквото броим
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        // ====== ЛОГИКА ТУК ======
        result += graph[node].size();
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
    
    // Пример: брой components или edges
    vector<int> results;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            results.push_back(bfs(i));
        }
    }
    
    // Output
    sort(results.begin(), results.end());
    for (int r : results) {
        cout << r << " ";
    }
    
    return 0;
}
```

---

## ВЪЗМОЖНИ ВАРИАЦИИ

### 1. Брой ребра в компонент
```cpp
int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int edgeCount = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        edgeCount += graph[node].size();  // всяко ребро се брои 2 пъти
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return edgeCount / 2;  // ВАЖНО!
}
```

### 2. Брой върхове в компонент
```cpp
int bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int nodeCount = 0;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        nodeCount++;
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push(adj);
            }
        }
    }
    
    return nodeCount;
}
```

### 3. Брой свързани компоненти
```cpp
int countComponents(int V) {
    int count = 0;
    for (int i = 0; i < V; i++) {
        if (!visited.count(i)) {
            bfs(i);
            count++;
        }
    }
    return count;
}
```

### 4. Най-кратък път (непретеглен граф)
```cpp
int bfs(int start, int target) {
    queue<pair<int, int>> q;  // {node, distance}
    q.push({start, 0});
    visited.insert(start);
    
    while (!q.empty()) {
        auto [node, dist] = q.front();
        q.pop();
        
        if (node == target) return dist;
        
        for (int adj : graph[node]) {
            if (!visited.count(adj)) {
                visited.insert(adj);
                q.push({adj, dist + 1});
            }
        }
    }
    
    return -1;  // няма път
}
```

### 5. BFS с Level (нива)
```cpp
void bfs(int start) {
    queue<int> q;
    q.push(start);
    visited.insert(start);
    
    int level = 0;
    
    while (!q.empty()) {
        int size = q.size();  // ВАЖНО: размер на текущото ниво
        
        for (int i = 0; i < size; i++) {
            int node = q.front();
            q.pop();
            
            // node е на ниво 'level'
            
            for (int adj : graph[node]) {
                if (!visited.count(adj)) {
                    visited.insert(adj);
                    q.push(adj);
                }
            }
        }
        
        level++;
    }
}
```

---

## HASH MAP ОПЕРАЦИИ

```cpp
unordered_map<int, int> mp;
mp[key] = value;        // insert/update
mp.count(key);          // 0 или 1
mp.erase(key);          // изтрий
mp[key]++;              // увеличи (default 0)

unordered_set<int> st;
st.insert(x);           // добави
st.count(x);            // 0 или 1
st.erase(x);            // изтрий

// Итерация
for (auto& [key, value] : mp) { ... }
for (int x : st) { ... }
```

---

## ПРИМЕРНИ ЗАДАЧИ ЗА HASHMAP + BFS

### Пример 1: Ребра по компоненти (от контролно)
```cpp
int main() {
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
            results.push_back(bfs(i));  // връща edgeCount / 2
        }
    }
    
    sort(results.begin(), results.end());
    for (int r : results) cout << r << " ";
}
```

### Пример 2: K-th Smallest с Max Heap (от контролно)
```cpp
int main() {
    int k;
    cin >> k;
    
    priority_queue<int> maxHeap;  // MAX heap
    
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
}
```

### Пример 3: Frequency Counting
```cpp
int main() {
    int n;
    cin >> n;
    
    unordered_map<int, int> freq;
    
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        freq[x]++;
    }
    
    // Намери най-честия
    int maxFreq = 0, result = 0;
    for (auto& [val, count] : freq) {
        if (count > maxFreq) {
            maxFreq = count;
            result = val;
        }
    }
    
    cout << result << endl;
}
```

---

# 🚨 ЧЕСТИ ГРЕШКИ

## Dijkstra Грешки

| Грешка | Грешен код | Правилен код |
|--------|------------|--------------|
| MAX heap вместо MIN | `priority_queue<pli> pq;` | `priority_queue<pli, vector<pli>, greater<pli>> pq;` |
| Липсва skip check | - | `if (d > dist[u]) continue;` |
| `int` overflow | `int dist[N];` | `long long dist[N];` |
| INF твърде малко | `const int INF = 1e6;` | `const ll INF = 1e18;` |
| Забравен `pq.pop()` | само `pq.top()` | `pq.top(); pq.pop();` |

## BFS Грешки

| Грешка | Грешен код | Правилен код |
|--------|------------|--------------|
| Visited след push | `q.push(); visited.insert();` | `visited.insert(); q.push();` |
| Забравен visited | без проверка | `if (!visited.count(adj))` |
| Грешен edge count | `return edgeCount;` | `return edgeCount / 2;` |

## Input Грешки

| Грешка | Проблем | Решение |
|--------|---------|---------|
| 1-indexed | `graph[0]` не съществува | `graph.resize(n+1)` или `u--; v--;` |
| Насочен/Ненасочен | Само едната посока | Провери условието |

---

# ⏱ ВРЕМЕВО РАЗПРЕДЕЛЕНИЕ

## 90 минути за 2 задачи

| Време | Действие |
|-------|----------|
| 0-5 мин | Прочети ДВЕТЕ задачи |
| 5-10 мин | Избери по-лесната, напиши header |
| 10-40 мин | Реши първата задача |
| 40-45 мин | Тествай, submit първата |
| 45-80 мин | Реши втората задача |
| 80-90 мин | Тествай, submit втората |

---

# 📋 CHECKLIST ПРЕДИ SUBMIT

## Dijkstra
- [ ] `greater<>` за MIN heap?
- [ ] `if (d > dist[u]) continue;` ?
- [ ] `long long` за дистанции?
- [ ] Правилен index (0 vs 1)?
- [ ] Насочен или ненасочен граф?

## BFS + HashMap
- [ ] `visited.insert()` ПРЕДИ `q.push()`?
- [ ] Edge count / 2 ако е ненасочен?
- [ ] Всички компоненти обходени?

---

# 🏆 QUICK REFERENCE

```
╔═══════════════════════════════════════════════════════════════╗
║                         DIJKSTRA                              ║
╠═══════════════════════════════════════════════════════════════╣
║  priority_queue<pli, vector<pli>, greater<pli>> pq;           ║
║  pq.push({0, start});                                         ║
║  dist[start] = 0;                                             ║
║                                                               ║
║  while (!pq.empty()) {                                        ║
║      auto [d, u] = pq.top(); pq.pop();                        ║
║      if (d > dist[u]) continue;                               ║
║                                                               ║
║      for (auto& [v, w] : graph[u]) {                          ║
║          ll newDist = /* СМЕТКА */;                           ║
║          if (newDist < dist[v]) {                             ║
║              dist[v] = newDist;                               ║
║              pq.push({dist[v], v});                           ║
║          }                                                    ║
║      }                                                        ║
║  }                                                            ║
╠═══════════════════════════════════════════════════════════════╣
║  СМЕТКИ:                                                      ║
║  • Standard:   dist[u] + w                                    ║
║  • Wait time:  dist[u] + wait + w                             ║
║  • Max path:   max(dist[u], w)                                ║
║  • With tax:   dist[u] + w + tax[v]                           ║
╚═══════════════════════════════════════════════════════════════╝

╔═══════════════════════════════════════════════════════════════╗
║                      BFS + HASHMAP                            ║
╠═══════════════════════════════════════════════════════════════╣
║  queue<int> q;                                                ║
║  q.push(start);                                               ║
║  visited.insert(start);                                       ║
║                                                               ║
║  while (!q.empty()) {                                         ║
║      int node = q.front(); q.pop();                           ║
║                                                               ║
║      /* ЛОГИКА */                                             ║
║                                                               ║
║      for (int adj : graph[node]) {                            ║
║          if (!visited.count(adj)) {                           ║
║              visited.insert(adj);                             ║
║              q.push(adj);                                     ║
║          }                                                    ║
║      }                                                        ║
║  }                                                            ║
╚═══════════════════════════════════════════════════════════════╝
```

---

**УСПЕХ НА ИЗПИТА! 🎯**
