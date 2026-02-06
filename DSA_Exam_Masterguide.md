# DSA Final Exam Masterguide (C++)

---

## Table of Contents

1. [Exam Pattern Analysis](#exam-pattern-analysis)
2. [How to Think When You See a Task](#how-to-think-when-you-see-a-task)
3. [C++ STL Cheat Sheet](#c-stl-cheat-sheet)
4. [Template 1: Union-Find (Disjoint Set Union)](#template-1-union-find-disjoint-set-union)
5. [Template 2: BFS (Breadth-First Search)](#template-2-bfs-breadth-first-search)
6. [Template 3: DFS (Depth-First Search)](#template-3-dfs-depth-first-search)
7. [Template 4: Cycle Detection](#template-4-cycle-detection)
8. [Template 5: Kruskal's MST](#template-5-kruskals-mst)
9. [Template 6: Prim's MST](#template-6-prims-mst)
10. [Template 7: Dijkstra's Shortest Path](#template-7-dijkstras-shortest-path)
11. [Template 8: Topological Sort](#template-8-topological-sort)
12. [Template 9: Binary Search](#template-9-binary-search)
13. [Template 10: Priority Queue / Heap](#template-10-priority-queue--heap)
14. [Template 11: Hash Map Patterns](#template-11-hash-map-patterns)
15. [Template 12: Prefix Sum + Hash Map (Subarray Sum = K)](#template-12-prefix-sum--hash-map-subarray-sum--k)
16. [Template 13: Dynamic Programming](#template-13-dynamic-programming)
17. [Template 14: Sorting with Custom Comparators](#template-14-sorting-with-custom-comparators)
18. [Template 15: Sliding Window / Two Pointers](#template-15-sliding-window--two-pointers)
19. [Template 16: Tree Traversals](#template-16-tree-traversals)
20. [Template 17: Graph Input Reading](#template-17-graph-input-reading)
21. [Template 18: Starter Boilerplate](#template-18-starter-boilerplate)
22. [Study Plan](#study-plan)
23. [Predicted 2026 Exam Structure](#predicted-2026-exam-structure)

---

## Exam Pattern Analysis

### Format
- **5 tasks** on HackerRank, C++ allowed
- Covers the full course (Sem01-Sem13), heavy emphasis on second half (graphs, heaps, MST, DP)

### What appeared on every final exam (2023, 2024, 2025)

| Slot | 2023 | 2024 | 2025 |
|------|------|------|------|
| **T1** | Hash Map simulation | Hash Map frequency analysis | Custom sorting |
| **T2** | Connected components (BFS/DFS/UF) | Binary Search (descending array) | Graph is-a-tree (DFS) |
| **T3** | Priority Queue (heap simulation) | Cycle detection (DFS/UF) | Prefix sums + Hash Map |
| **T4** | BFS shortest path with constraints | MST (Kruskal/Prim) | K-Clustering (UF/Kruskal) |
| **T5** | Binary Search + sorting (queries) | Tree DFS greedy (min dominating set) | DP (LCS) |

### Topic frequency across all 3 years

| Topic | 2023 | 2024 | 2025 | Priority |
|-------|------|------|------|----------|
| Union-Find | yes | yes | yes | CRITICAL |
| BFS/DFS graph traversal | yes | yes | yes | CRITICAL |
| Hash Maps / frequency | yes | yes | yes | CRITICAL |
| MST (Kruskal/Prim) | yes | yes | yes | CRITICAL |
| Binary Search | yes | yes | - | HIGH |
| Priority Queue / Heap | yes | yes | - | HIGH |
| Tree DFS | - | yes | yes | HIGH |
| Dynamic Programming | - | - | yes | HIGH |
| Custom Sorting | - | - | yes | MEDIUM |
| Prefix Sums | - | - | yes | MEDIUM |
| Sliding Window | midterms | midterms | - | MEDIUM |

---

## How to Think When You See a Task

### Step 1: Read the problem and identify the INPUT

Ask yourself:
- Am I given a **graph** (nodes + edges)? -> BFS, DFS, Union-Find, MST, Dijkstra
- Am I given an **array/sequence**? -> Sorting, Binary Search, Sliding Window, Prefix Sum, DP
- Am I given a **tree**? -> DFS/BFS traversal, Tree DP
- Am I given **strings**? -> Hash Map, frequency counting, DP (LCS)
- Am I given **queries** over data? -> Precompute + Binary Search, or Prefix Sums

### Step 2: Identify WHAT they're asking

| They ask for... | Think about... |
|-----------------|---------------|
| "Count connected components" | BFS/DFS or Union-Find |
| "Is it a tree?" | DFS: connected + no cycles (V-1 edges) |
| "Components with property X" | Union-Find with metadata per component |
| "Shortest path (unweighted)" | BFS |
| "Shortest path (weighted, positive)" | Dijkstra |
| "Minimum total cost to connect" | MST (Kruskal or Prim) |
| "Group into K clusters" | Kruskal stopped early (K components left) |
| "Cycle in undirected graph" | DFS with parent OR Union-Find |
| "Cycle in directed graph" | DFS with coloring (white/gray/black) |
| "Number of subarrays with sum K" | Prefix sum + hash map |
| "Longest subarray with property" | Sliding window |
| "K-th largest/smallest" | Heap (priority_queue) |
| "Sort by complex criteria" | std::sort with custom comparator |
| "Closest element / insertion point" | Binary search (lower_bound/upper_bound) |
| "Longest common subsequence" | 2D DP |
| "Min cost path in grid" | 2D DP |
| "Min coins to make amount" | 1D DP (Coin Change) |
| "Ordering with dependencies" | Topological sort |
| "Process events in time order" | Sort events + heap simulation |

### Step 3: Check constraints (N size)

| N | Acceptable complexity | Approach |
|---|----------------------|----------|
| N <= 20 | O(2^N), O(N!) | Brute force, backtracking |
| N <= 1000 | O(N^2) | Nested loops, 2D DP |
| N <= 100,000 | O(N log N) | Sorting, binary search, heap |
| N <= 1,000,000 | O(N) or O(N * alpha(N)) | Hash map, Union-Find, prefix sum |

---

## C++ STL Cheat Sheet

### Headers you will always need

```cpp
#include <bits/stdc++.h>  // includes EVERYTHING (use on competitive/exam)
using namespace std;
```

If `bits/stdc++.h` is not available:

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <numeric>
#include <climits>
#include <cmath>
using namespace std;
```

### Fast I/O (put at start of main, ALWAYS)

```cpp
ios_base::sync_with_stdio(false);
cin.tie(nullptr);
```

### std::vector

```cpp
vector<int> v(n);               // size n, default 0
vector<int> v(n, val);           // size n, all = val
vector<vector<int>> adj(n);      // adjacency list
v.push_back(x);                  // add to end O(1) amortized
v.pop_back();                    // remove last O(1)
v.size();                        // number of elements
v.empty();                       // is empty?
v[i];                            // access by index O(1)
v.front(); v.back();             // first/last element
v.begin(); v.end();              // iterators
v.clear();                       // remove all
v.resize(n);                     // resize
sort(v.begin(), v.end());        // sort ascending
sort(v.begin(), v.end(), greater<int>()); // sort descending
reverse(v.begin(), v.end());     // reverse
```

### std::string

```cpp
string s;
cin >> s;                        // read word (no spaces)
getline(cin, s);                 // read full line
s.length(); s.size();            // length
s[i];                            // char at index
s.substr(pos, len);              // substring
s.find("abc");                   // find substring, returns npos if not found
s += "abc";                      // append
to_string(42);                   // int -> string
stoi("42");                      // string -> int
stoll("123456789");              // string -> long long
```

### std::unordered_map (Hash Map)

```cpp
unordered_map<string, int> mp;
mp["key"] = value;               // insert/update
mp["key"];                       // access (creates with 0 if missing!)
mp.count("key");                 // 1 if exists, 0 if not
mp.find("key");                  // iterator, == mp.end() if not found
mp.erase("key");                 // remove
mp.size();                       // number of entries
for (auto& [key, val] : mp) {}  // iterate
```

### std::unordered_set

```cpp
unordered_set<int> st;
st.insert(x);                   // add
st.erase(x);                    // remove
st.count(x);                    // 1 if exists, 0 if not
st.find(x);                     // iterator
st.size();
```

### std::map (ordered, Red-Black Tree)

```cpp
map<int, int> mp;                // keys sorted ascending
mp[key] = val;
mp.lower_bound(x);              // iterator to first key >= x
mp.upper_bound(x);              // iterator to first key > x
mp.begin();                      // smallest key
mp.rbegin();                     // largest key
```

### std::set (ordered)

```cpp
set<int> st;
st.insert(x);
st.erase(x);
st.count(x);
st.lower_bound(x);              // iterator to first >= x
st.upper_bound(x);              // iterator to first > x
*st.begin();                     // smallest element
*st.rbegin();                    // largest element
```

### std::priority_queue (Heap)

```cpp
priority_queue<int> maxHeap;                           // MAX heap (biggest on top)
priority_queue<int, vector<int>, greater<int>> minHeap; // MIN heap (smallest on top)
maxHeap.push(x);                // insert O(log n)
maxHeap.top();                  // peek O(1)
maxHeap.pop();                  // remove top O(log n)
maxHeap.size();
maxHeap.empty();
```

### std::queue

```cpp
queue<int> q;
q.push(x);                     // add to back
q.front();                      // peek front
q.pop();                        // remove front
q.size();
q.empty();
```

### std::stack

```cpp
stack<int> st;
st.push(x);                    // add to top
st.top();                       // peek top
st.pop();                       // remove top
st.size();
st.empty();
```

### std::pair and std::tuple

```cpp
pair<int, int> p = {a, b};
p.first; p.second;
auto [x, y] = p;               // structured binding (C++17)

tuple<int, int, int> t = {a, b, c};
auto [x, y, z] = t;
get<0>(t); get<1>(t); get<2>(t);
```

### Useful algorithms

```cpp
sort(v.begin(), v.end());
stable_sort(v.begin(), v.end());
reverse(v.begin(), v.end());
min(a, b); max(a, b);
min_element(v.begin(), v.end());
max_element(v.begin(), v.end());
accumulate(v.begin(), v.end(), 0LL);  // sum (use 0LL for long long)
count(v.begin(), v.end(), val);
fill(v.begin(), v.end(), val);
unique(v.begin(), v.end());           // remove consecutive dups (sort first)
lower_bound(v.begin(), v.end(), x);   // first >= x (sorted array)
upper_bound(v.begin(), v.end(), x);   // first > x  (sorted array)
next_permutation(v.begin(), v.end());
swap(a, b);
abs(x);
```

---

## Template 1: Union-Find (Disjoint Set Union)

**When to use:** Connected components, cycle detection, Kruskal's MST, clustering, merging groups with metadata.

**CRITICAL -- this appears on EVERY exam.**

```cpp
struct DSU {
    vector<int> parent, rank_;
    int components;

    DSU(int n) : parent(n), rank_(n, 0), components(n) {
        iota(parent.begin(), parent.end(), 0); // parent[i] = i
    }

    int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]]; // path compression
            x = parent[x];
        }
        return x;
    }

    bool unite(int a, int b) {
        a = find(a);
        b = find(b);
        if (a == b) return false; // already same component (cycle if edge!)
        if (rank_[a] < rank_[b]) swap(a, b);
        parent[b] = a;
        if (rank_[a] == rank_[b]) rank_[a]++;
        components--;
        return true;
    }

    bool connected(int a, int b) {
        return find(a) == find(b);
    }
};
```

### Union-Find with component SIZE tracking (common exam variant)

```cpp
struct DSU {
    vector<int> parent, sz;
    int components;

    DSU(int n) : parent(n), sz(n, 1), components(n) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    bool unite(int a, int b) {
        a = find(a); b = find(b);
        if (a == b) return false;
        if (sz[a] < sz[b]) swap(a, b);
        parent[b] = a;
        sz[a] += sz[b];
        components--;
        return true;
    }

    int getSize(int x) { return sz[find(x)]; }
};
```

### Union-Find with metadata (e.g., max edge weight per component)

```cpp
struct DSU {
    vector<int> parent, rank_;
    vector<long long> maxWeight; // or any other per-component data

    DSU(int n) : parent(n), rank_(n, 0), maxWeight(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    void unite(int a, int b, long long w) {
        a = find(a); b = find(b);
        if (a == b) {
            maxWeight[a] = max(maxWeight[a], w);
            return;
        }
        if (rank_[a] < rank_[b]) swap(a, b);
        parent[b] = a;
        if (rank_[a] == rank_[b]) rank_[a]++;
        maxWeight[a] = max({maxWeight[a], maxWeight[b], w});
    }
};
```

---

## Template 2: BFS (Breadth-First Search)

**When to use:** Shortest path in unweighted graphs, level-order traversal, connected components, "minimum steps" problems.

```cpp
// BFS from a source node, returns distance array
vector<int> bfs(int src, const vector<vector<int>>& adj, int n) {
    vector<int> dist(n, -1);
    queue<int> q;
    dist[src] = 0;
    q.push(src);

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        for (int nei : adj[node]) {
            if (dist[nei] == -1) {
                dist[nei] = dist[node] + 1;
                q.push(nei);
            }
        }
    }
    return dist;
}
```

### BFS to count connected components

```cpp
int countComponents(const vector<vector<int>>& adj, int n) {
    vector<bool> visited(n, false);
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            count++;
            queue<int> q;
            q.push(i);
            visited[i] = true;
            while (!q.empty()) {
                int node = q.front(); q.pop();
                for (int nei : adj[node]) {
                    if (!visited[nei]) {
                        visited[nei] = true;
                        q.push(nei);
                    }
                }
            }
        }
    }
    return count;
}
```

### BFS with forbidden/blocked nodes

```cpp
int bfsWithBlocked(int src, int dst, const vector<vector<int>>& adj,
                   int n, const unordered_set<int>& blocked) {
    if (src == dst) return 0;
    vector<int> dist(n, -1);
    queue<int> q;
    dist[src] = 0;
    q.push(src);

    while (!q.empty()) {
        int node = q.front(); q.pop();
        for (int nei : adj[node]) {
            if (dist[nei] == -1 && !blocked.count(nei)) {
                dist[nei] = dist[node] + 1;
                if (nei == dst) return dist[nei];
                q.push(nei);
            }
        }
    }
    return -1; // unreachable
}
```

---

## Template 3: DFS (Depth-First Search)

**When to use:** Cycle detection, path finding, connected components, tree problems, topological sort.

### Iterative DFS (avoids stack overflow)

```cpp
void dfs(int src, const vector<vector<int>>& adj, vector<bool>& visited) {
    stack<int> st;
    st.push(src);
    visited[src] = true;

    while (!st.empty()) {
        int node = st.top(); st.pop();
        for (int nei : adj[node]) {
            if (!visited[nei]) {
                visited[nei] = true;
                st.push(nei);
            }
        }
    }
}
```

### Recursive DFS

```cpp
void dfs(int node, const vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    for (int nei : adj[node]) {
        if (!visited[nei]) {
            dfs(nei, adj, visited);
        }
    }
}
```

### DFS that returns component size

```cpp
int dfsSize(int node, const vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    int size = 1;
    for (int nei : adj[node]) {
        if (!visited[nei]) {
            size += dfsSize(nei, adj, visited);
        }
    }
    return size;
}
```

---

## Template 4: Cycle Detection

### Undirected graph -- DFS with parent

```cpp
bool hasCycleDFS(int node, int parent, const vector<vector<int>>& adj,
                 vector<bool>& visited) {
    visited[node] = true;
    for (int nei : adj[node]) {
        if (!visited[nei]) {
            if (hasCycleDFS(nei, node, adj, visited)) return true;
        } else if (nei != parent) {
            return true; // back edge = cycle
        }
    }
    return false;
}

// Usage:
// vector<bool> visited(n, false);
// bool cycle = hasCycleDFS(0, -1, adj, visited);
```

### Undirected graph -- Union-Find (simpler)

```cpp
bool hasCycleUF(int n, vector<pair<int,int>>& edges) {
    DSU dsu(n);
    for (auto& [u, v] : edges) {
        if (!dsu.unite(u, v)) return true; // same component = cycle
    }
    return false;
}
```

### Directed graph -- DFS with coloring

```cpp
// 0 = white (unvisited), 1 = gray (in stack), 2 = black (done)
bool hasCycleDirected(int node, const vector<vector<int>>& adj,
                      vector<int>& color) {
    color[node] = 1;
    for (int nei : adj[node]) {
        if (color[nei] == 1) return true;       // back edge
        if (color[nei] == 0 && hasCycleDirected(nei, adj, color)) return true;
    }
    color[node] = 2;
    return false;
}
```

### Check if undirected graph is a tree

```cpp
bool isTree(int n, const vector<vector<int>>& adj) {
    // A tree: connected + exactly n-1 edges + no cycles
    // Simplest: DFS from 0, check all visited and no cycle
    vector<bool> visited(n, false);
    bool cycle = hasCycleDFS(0, -1, adj, visited);
    if (cycle) return false;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) return false; // not connected
    }
    return true;
}
```

---

## Template 5: Kruskal's MST

**When to use:** MST, clustering, "minimum cost to connect all nodes", components with edge-weight tracking.

```cpp
struct Edge {
    int u, v;
    long long w;
    bool operator<(const Edge& o) const { return w < o.w; }
};

long long kruskal(int n, vector<Edge>& edges) {
    sort(edges.begin(), edges.end());
    DSU dsu(n);
    long long mstWeight = 0;
    int edgesUsed = 0;

    for (auto& [u, v, w] : edges) {
        if (dsu.unite(u, v)) {
            mstWeight += w;
            edgesUsed++;
            if (edgesUsed == n - 1) break; // MST complete
        }
    }
    return mstWeight;
}
```

### Kruskal's K-Clustering (stop when K components remain)

```cpp
// Returns when exactly K clusters remain
void kCluster(int n, int k, vector<Edge>& edges, DSU& dsu) {
    sort(edges.begin(), edges.end());
    for (auto& [u, v, w] : edges) {
        if (dsu.components == k) break; // done
        dsu.unite(u, v);
    }
    // dsu now has exactly k components
}
```

### Kruskal's MST per component with size filter

```cpp
// Sum MST weights only for components with size divisible by K
long long kruskalFiltered(int n, vector<Edge>& edges, int K) {
    sort(edges.begin(), edges.end());
    DSU dsu(n); // use the version with sz[]
    vector<long long> compWeight(n, 0);

    for (auto& [u, v, w] : edges) {
        int pu = dsu.find(u), pv = dsu.find(v);
        if (pu != pv) {
            long long combined = compWeight[pu] + compWeight[pv] + w;
            dsu.unite(u, v);
            compWeight[dsu.find(u)] = combined;
        }
    }

    long long total = 0;
    for (int i = 0; i < n; i++) {
        if (dsu.find(i) == i && dsu.getSize(i) % K == 0) {
            total += compWeight[i];
        }
    }
    return total;
}
```

---

## Template 6: Prim's MST

**When to use:** Same as Kruskal's, but better when graph is dense (adjacency matrix/list).

```cpp
long long prim(int n, const vector<vector<pair<int, long long>>>& adj) {
    vector<bool> inMST(n, false);
    // {weight, node}
    priority_queue<pair<long long, int>, vector<pair<long long, int>>,
                   greater<pair<long long, int>>> pq;
    pq.push({0, 0});
    long long mstWeight = 0;
    int edgesUsed = 0;

    while (!pq.empty() && edgesUsed < n) {
        auto [w, u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        mstWeight += w;
        edgesUsed++;

        for (auto& [v, weight] : adj[u]) {
            if (!inMST[v]) {
                pq.push({weight, v});
            }
        }
    }
    return mstWeight;
}
```

---

## Template 7: Dijkstra's Shortest Path

**When to use:** Shortest path in weighted graph with NON-NEGATIVE edges.

```cpp
vector<long long> dijkstra(int src, int n,
                           const vector<vector<pair<int, long long>>>& adj) {
    vector<long long> dist(n, LLONG_MAX);
    // {distance, node}
    priority_queue<pair<long long, int>, vector<pair<long long, int>>,
                   greater<pair<long long, int>>> pq;
    dist[src] = 0;
    pq.push({0, src});

    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue; // outdated entry
        for (auto& [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

---

## Template 8: Topological Sort

**When to use:** Ordering with dependencies (Course Schedule), DAG shortest path.

### Kahn's Algorithm (BFS-based, gives topological order)

```cpp
vector<int> topSort(int n, const vector<vector<int>>& adj) {
    vector<int> indegree(n, 0);
    for (int u = 0; u < n; u++)
        for (int v : adj[u])
            indegree[v]++;

    queue<int> q;
    for (int i = 0; i < n; i++)
        if (indegree[i] == 0)
            q.push(i);

    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u]) {
            if (--indegree[v] == 0)
                q.push(v);
        }
    }
    // if order.size() != n, there's a cycle
    return order;
}
```

### DFS-based Topological Sort

```cpp
void dfsTopo(int node, const vector<vector<int>>& adj,
             vector<bool>& visited, stack<int>& stk) {
    visited[node] = true;
    for (int nei : adj[node])
        if (!visited[nei])
            dfsTopo(nei, adj, visited, stk);
    stk.push(node); // post-order
}

vector<int> topSortDFS(int n, const vector<vector<int>>& adj) {
    vector<bool> visited(n, false);
    stack<int> stk;
    for (int i = 0; i < n; i++)
        if (!visited[i])
            dfsTopo(i, adj, visited, stk);

    vector<int> order;
    while (!stk.empty()) {
        order.push_back(stk.top());
        stk.pop();
    }
    return order;
}
```

---

## Template 9: Binary Search

**When to use:** Sorted data, "find minimum X such that...", closest element, insertion point.

### Standard binary search

```cpp
int binarySearch(const vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1; // not found
}
```

### Binary search on answer space ("minimum X such that condition is true")

```cpp
// Example: minimum speed to eat all food in H hours
int binarySearchAnswer(const vector<int>& arr, int H) {
    int lo = 1, hi = *max_element(arr.begin(), arr.end());
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (feasible(arr, mid, H)) hi = mid;     // mid works, try smaller
        else lo = mid + 1;                        // mid too small
    }
    return lo;
}
```

### Using STL lower_bound / upper_bound

```cpp
// arr MUST be sorted ascending
auto it = lower_bound(arr.begin(), arr.end(), x); // first >= x
auto it2 = upper_bound(arr.begin(), arr.end(), x); // first > x
int idx = it - arr.begin();                        // convert to index

// Count elements in range [lo, hi]:
int cnt = upper_bound(arr.begin(), arr.end(), hi)
        - lower_bound(arr.begin(), arr.end(), lo);

// Closest element to x in sorted array:
auto it = lower_bound(arr.begin(), arr.end(), x);
int closest;
if (it == arr.end()) closest = arr.back();
else if (it == arr.begin()) closest = arr.front();
else {
    int a = *prev(it), b = *it;
    closest = (x - a <= b - x) ? a : b;
}
```

---

## Template 10: Priority Queue / Heap

**When to use:** K-th largest/smallest, merge K sorted things, event simulation, Dijkstra, Prim.

### Min-heap and Max-heap

```cpp
// Min-heap (smallest on top)
priority_queue<int, vector<int>, greater<int>> minH;

// Max-heap (largest on top, default)
priority_queue<int> maxH;
```

### K-th largest element (maintain min-heap of size K)

```cpp
int kthLargest(const vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minH;
    for (int x : nums) {
        minH.push(x);
        if ((int)minH.size() > k) minH.pop();
    }
    return minH.top(); // k-th largest
}
```

### Merge K sorted lists

```cpp
// {value, list_index, element_index}
priority_queue<tuple<int,int,int>, vector<tuple<int,int,int>>,
               greater<tuple<int,int,int>>> pq;

// Push first element of each list
for (int i = 0; i < k; i++)
    if (!lists[i].empty())
        pq.push({lists[i][0], i, 0});

vector<int> result;
while (!pq.empty()) {
    auto [val, li, ei] = pq.top(); pq.pop();
    result.push_back(val);
    if (ei + 1 < (int)lists[li].size())
        pq.push({lists[li][ei + 1], li, ei + 1});
}
```

### Event-driven simulation (process by time)

```cpp
// Example: chairs problem - events sorted by time
// {time, type (+1=arrive, -1=depart), person_id}
priority_queue<tuple<int,int,int>, vector<tuple<int,int,int>>,
               greater<>> events;

// Available chairs (min-heap gives lowest numbered)
priority_queue<int, vector<int>, greater<int>> chairs;
for (int i = 0; i < n; i++) chairs.push(i);
```

---

## Template 11: Hash Map Patterns

### Frequency counting

```cpp
unordered_map<int, int> freq;
for (int x : arr) freq[x]++;

// Find most frequent:
int maxFreq = 0, mostCommon = 0;
for (auto& [val, cnt] : freq) {
    if (cnt > maxFreq) {
        maxFreq = cnt;
        mostCommon = val;
    }
}
```

### Frequency of frequencies

```cpp
unordered_map<char, int> charFreq;
for (char c : s) charFreq[c]++;

unordered_map<int, int> freqOfFreq;
for (auto& [ch, f] : charFreq) freqOfFreq[f]++;
// Now freqOfFreq[3] = how many characters appear exactly 3 times
```

### Two Sum pattern (find pair with target sum)

```cpp
unordered_map<int, int> seen; // value -> index
for (int i = 0; i < n; i++) {
    int complement = target - arr[i];
    if (seen.count(complement)) {
        // found pair: seen[complement] and i
    }
    seen[arr[i]] = i;
}
```

### Group by key (like Group Anagrams)

```cpp
unordered_map<string, vector<string>> groups;
for (const string& s : words) {
    string key = s;
    sort(key.begin(), key.end());
    groups[key].push_back(s);
}
```

---

## Template 12: Prefix Sum + Hash Map (Subarray Sum = K)

**When to use:** "Count subarrays with sum = K", "Longest subarray with sum = K"

### Count subarrays with sum = K

```cpp
int subarraySum(const vector<int>& arr, int k) {
    unordered_map<long long, int> prefixCount;
    prefixCount[0] = 1;
    long long sum = 0;
    int count = 0;

    for (int x : arr) {
        sum += x;
        if (prefixCount.count(sum - k))
            count += prefixCount[sum - k];
        prefixCount[sum]++;
    }
    return count;
}
```

### Longest subarray with sum = K

```cpp
int longestSubarraySum(const vector<int>& arr, int k) {
    unordered_map<long long, int> firstOccurrence;
    firstOccurrence[0] = -1; // prefix sum 0 at index -1
    long long sum = 0;
    int maxLen = 0;

    for (int i = 0; i < (int)arr.size(); i++) {
        sum += arr[i];
        if (firstOccurrence.count(sum - k))
            maxLen = max(maxLen, i - firstOccurrence[sum - k]);
        if (!firstOccurrence.count(sum))
            firstOccurrence[sum] = i; // only store FIRST occurrence
    }
    return maxLen;
}
```

---

## Template 13: Dynamic Programming

### 1D DP -- Fibonacci

```cpp
int fib(int n) {
    if (n <= 1) return n;
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### 1D DP -- Coin Change (minimum coins to make amount)

```cpp
int coinChange(const vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, INT_MAX);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int c : coins) {
            if (c <= i && dp[i - c] != INT_MAX) {
                dp[i] = min(dp[i], dp[i - c] + 1);
            }
        }
    }
    return dp[amount] == INT_MAX ? -1 : dp[amount];
}
```

### 1D DP -- House Robber

```cpp
int rob(const vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return 0;
    if (n == 1) return nums[0];
    int prev2 = nums[0], prev1 = max(nums[0], nums[1]);
    for (int i = 2; i < n; i++) {
        int curr = max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### 1D DP -- Longest Increasing Subsequence (O(n^2))

```cpp
int LIS(const vector<int>& arr) {
    int n = arr.size();
    vector<int> dp(n, 1);
    int ans = 1;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (arr[j] < arr[i])
                dp[i] = max(dp[i], dp[j] + 1);
        }
        ans = max(ans, dp[i]);
    }
    return ans;
}
```

### 2D DP -- Longest Common Subsequence

```cpp
int LCS(const string& s1, const string& s2) {
    int m = s1.size(), n = s2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i - 1] == s2[j - 1])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[m][n];
}
```

### 2D DP -- Grid Path (min cost / max value from top-left to bottom-right)

```cpp
// Move only right or down
int gridDP(const vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    vector<vector<int>> dp(n, vector<int>(m, 0));
    dp[0][0] = grid[0][0];

    for (int i = 1; i < n; i++) dp[i][0] = dp[i-1][0] + grid[i][0];
    for (int j = 1; j < m; j++) dp[0][j] = dp[0][j-1] + grid[0][j];

    for (int i = 1; i < n; i++)
        for (int j = 1; j < m; j++)
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            // use max() if maximizing

    return dp[n-1][m-1];
}
```

### 2D DP -- Unique Paths

```cpp
int uniquePaths(int m, int n) {
    vector<vector<int>> dp(m, vector<int>(n, 1));
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
    return dp[m-1][n-1];
}
```

---

## Template 14: Sorting with Custom Comparators

**When to use:** "Sort by X, break ties by Y", custom ordering.

### Lambda comparator

```cpp
// Sort by second element descending, then first ascending
vector<pair<int,int>> v;
sort(v.begin(), v.end(), [](const pair<int,int>& a, const pair<int,int>& b) {
    if (a.second != b.second) return a.second > b.second; // descending
    return a.first < b.first; // ascending tiebreak
});
```

### Sort indices by computed key

```cpp
vector<int> indices(n);
iota(indices.begin(), indices.end(), 0); // {0, 1, 2, ..., n-1}
sort(indices.begin(), indices.end(), [&](int i, int j) {
    double ki = (double)a[i] * a[i] / b[i];
    double kj = (double)a[j] * a[j] / b[j];
    if (ki != kj) return ki > kj;     // descending by key
    return a[i] > a[j];               // tiebreak
});
// indices now contains original positions in the desired order
```

### Stable sort (preserves relative order of equal elements)

```cpp
stable_sort(v.begin(), v.end(), comparator);
```

---

## Template 15: Sliding Window / Two Pointers

**When to use:** "Longest/shortest subarray with property X", "at most K distinct elements"

### Longest subarray where every element appears at most K times

```cpp
int longestSubarray(const vector<int>& arr, int k) {
    unordered_map<int, int> freq;
    int left = 0, maxLen = 0;

    for (int right = 0; right < (int)arr.size(); right++) {
        freq[arr[right]]++;
        while (freq[arr[right]] > k) {
            freq[arr[left]]--;
            if (freq[arr[left]] == 0) freq.erase(arr[left]);
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

### Longest substring without repeating characters

```cpp
int lengthOfLongestSubstring(const string& s) {
    unordered_set<char> window;
    int left = 0, maxLen = 0;

    for (int right = 0; right < (int)s.size(); right++) {
        while (window.count(s[right])) {
            window.erase(s[left]);
            left++;
        }
        window.insert(s[right]);
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

---

## Template 16: Tree Traversals

### Tree node structure

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};
```

### Reading a tree from input (common exam format: N nodes, each with left/right child index)

```cpp
int n;
cin >> n;
vector<int> leftChild(n), rightChild(n);
for (int i = 0; i < n; i++) {
    cin >> leftChild[i] >> rightChild[i];
    // -1 means no child
}
```

### Inorder, Preorder, Postorder (recursive)

```cpp
void inorder(int node, const vector<int>& left, const vector<int>& right,
             const vector<int>& val) {
    if (node == -1) return;
    inorder(left[node], left, right, val);
    cout << val[node] << " ";
    inorder(right[node], left, right, val);
}

void preorder(int node, ...) {
    if (node == -1) return;
    cout << val[node] << " ";
    preorder(left[node], ...);
    preorder(right[node], ...);
}

void postorder(int node, ...) {
    if (node == -1) return;
    postorder(left[node], ...);
    postorder(right[node], ...);
    cout << val[node] << " ";
}
```

### Level-order traversal (BFS)

```cpp
void levelOrder(int root, const vector<int>& left, const vector<int>& right,
                const vector<int>& val) {
    if (root == -1) return;
    queue<int> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size(); // nodes at this level
        for (int i = 0; i < sz; i++) {
            int node = q.front(); q.pop();
            cout << val[node] << " ";
            if (left[node] != -1) q.push(left[node]);
            if (right[node] != -1) q.push(right[node]);
        }
        cout << endl; // next level
    }
}
```

### Post-order DFS with state (minimum dominating set pattern)

```cpp
// States: 0 = needs coverage, 1 = is a "painter", 2 = already covered
int painters = 0;

int dfsState(int node, const vector<int>& left, const vector<int>& right) {
    if (node == -1) return 2; // null nodes are "covered"

    int l = dfsState(left[node], left, right);
    int r = dfsState(right[node], left, right);

    if (l == 0 || r == 0) {  // a child needs coverage -> become painter
        painters++;
        return 1;
    }
    if (l == 1 || r == 1) {  // a child is a painter -> I'm covered
        return 2;
    }
    return 0; // both children covered, I need coverage
}

// After calling: if root returns 0, painters++ (root needs coverage too)
```

---

## Template 17: Graph Input Reading

### Undirected graph from edge list

```cpp
int n, m;
cin >> n >> m;
vector<vector<int>> adj(n);
for (int i = 0; i < m; i++) {
    int u, v;
    cin >> u >> v;
    adj[u].push_back(v);
    adj[v].push_back(u);
}
```

### Directed graph from edge list

```cpp
int n, m;
cin >> n >> m;
vector<vector<int>> adj(n);
for (int i = 0; i < m; i++) {
    int u, v;
    cin >> u >> v;
    adj[u].push_back(v); // only one direction
}
```

### Weighted undirected graph

```cpp
int n, m;
cin >> n >> m;
vector<vector<pair<int, long long>>> adj(n);
for (int i = 0; i < m; i++) {
    int u, v;
    long long w;
    cin >> u >> v >> w;
    adj[u].push_back({v, w});
    adj[v].push_back({u, w});
}
```

### Weighted edge list (for Kruskal)

```cpp
int n, m;
cin >> n >> m;
vector<Edge> edges(m);
for (int i = 0; i < m; i++) {
    cin >> edges[i].u >> edges[i].v >> edges[i].w;
}
```

### Multiple test cases pattern

```cpp
int T;
cin >> T;
while (T--) {
    int n, m;
    cin >> n >> m;
    // ... solve one test case
}
```

---

## Template 18: Starter Boilerplate

Copy-paste this at the start of every task:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    // read input

    // solve

    // output

    return 0;
}
```

---

## Study Plan

### Week 1: Lock in the Guarantees (CRITICAL topics)

| Day | Focus | What to do |
|-----|-------|------------|
| 1 | Union-Find | Write DSU from memory 3 times. Solve: Number of Provinces, Clusters |
| 2 | BFS/DFS | Write both from memory. Solve: Cycle Detection, Connected Components, All Paths |
| 3 | Kruskal's MST | Combine DSU + sorting. Solve: Min Cost to Connect All Points |
| 4 | Hash Maps | Practice frequency counting. Solve: Two Sum, Subarray Sum = K, Group Anagrams |
| 5 | Combine | Do final_exam_2024 Task3 (cycle detection) + Task4 (MST) from scratch |

### Week 2: Strengthen (HIGH priority topics)

| Day | Focus | What to do |
|-----|-------|------------|
| 6 | Binary Search | lower_bound, search on answer. Solve: Search 2D Matrix, Sqrt(x) |
| 7 | Priority Queue | Min/max heap, K-th element. Solve: Kth Largest, Merge K Sorted Lists |
| 8 | Tree DFS | Traversals + state-based DFS. Solve: Binary Tree Cameras, Validate BST |
| 9 | DP | LCS, Coin Change, Grid DP. Solve: LCS, House Robber, Unique Paths |
| 10 | Dijkstra | Write from memory. Solve: Network Delay Time |

### Week 3: Simulate and Review

| Day | Focus | What to do |
|-----|-------|------------|
| 11 | Timed mock | Do final_exam_2025 (all 5 tasks, 3h time limit) |
| 12 | Review gaps | Re-do any tasks you couldn't solve |
| 13 | Timed mock | Do final_exam_2024 (all 5 tasks, 3h time limit) |
| 14 | Final review | Re-read this file. Write DSU + BFS + Kruskal from memory one last time |

---

## Predicted 2026 Exam Structure

| Task | Expected Topic | Difficulty | What to prepare |
|------|---------------|------------|-----------------|
| **T1** | Hash Map / Sorting / Strings | Easy-Medium | Frequency counting, custom sort |
| **T2** | Graph BFS/DFS | Medium | Connected components, tree check, shortest path |
| **T3** | Prefix sums + hash map OR heap | Medium | Subarray sum = K, K-th element, event sim |
| **T4** | MST / Union-Find variant | Medium-Hard | Kruskal with metadata, K-clustering, component filtering |
| **T5** | DP or Tree DFS greedy | Hard | LCS, grid DP, tree state propagation |

---

## Quick Reference: Complexity Table

| Algorithm | Time | Space |
|-----------|------|-------|
| BFS / DFS | O(V + E) | O(V) |
| Union-Find (with compression) | O(alpha(N)) per op | O(N) |
| Kruskal's MST | O(E log E) | O(V + E) |
| Prim's MST | O(E log V) | O(V + E) |
| Dijkstra | O(E log V) | O(V + E) |
| Binary Search | O(log N) | O(1) |
| Topological Sort | O(V + E) | O(V) |
| Prefix Sum build | O(N) | O(N) |
| Subarray Sum = K | O(N) | O(N) |
| Heap push/pop | O(log N) | - |
| Heap build | O(N) | - |
| LCS (DP) | O(N * M) | O(N * M) |
| Coin Change (DP) | O(N * amount) | O(amount) |
| LIS | O(N^2) or O(N log N) | O(N) |
| Sorting | O(N log N) | O(N) |
| Hash map lookup | O(1) average | O(N) |

---

**Good luck on the exam! Focus on Union-Find + BFS/DFS + Kruskal first -- they cover the most ground.**
