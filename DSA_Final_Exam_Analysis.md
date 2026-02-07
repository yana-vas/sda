# DSA Final Exam - Complete Analysis (2018-2025)

All 7 years of final exams analyzed. This guide covers every task, every pattern, and exactly what you need to know.

---

## Table of Contents

1. [Every Exam Task at a Glance (Master Table)](#1-every-exam-task-at-a-glance)
2. [Task Slot Patterns](#2-task-slot-patterns)
3. [Topic Frequency Across All 7 Years](#3-topic-frequency-across-all-7-years)
4. [Detailed Exam Breakdown by Year](#4-detailed-exam-breakdown-by-year)
5. [How to Think When You See a Task](#5-how-to-think-when-you-see-a-task)
6. [What to Expect in 2026](#6-what-to-expect-in-2026)
7. [Priority Study Order](#7-priority-study-order)

---

## 1. Every Exam Task at a Glance

### 2018-2019 (4 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Tree Operations Part 1 | BST | Insert + pre-order traversal | Easy |
| T2 | Tree Operations Part 2 | BST + BFS | Delete (3 cases) + print odd levels (BFS level-order) | Medium |
| T3 | Dundee The Crocodile | Hash Map | Word frequency counting, filter count==1, sort | Easy |
| T4 | Cyclic Graph | Directed graph | DFS cycle detection with recursion stack | Medium |

### 2019-2020 (5 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Road Check | Weighted graph | Path validation via adjacency map lookup | Easy |
| T2 | Find Element | Binary Search | First/last position + insertion point in sorted array | Medium |
| T3 | Counting Regions | Graph | DFS connected component counting | Medium |
| T4 | Tree Profile | BST + BFS | BST insert + left-side view via level-order BFS | Medium |
| T5 | Shortest Tour | BFS | Sequential BFS between waypoints, blocking future waypoints | Medium |

### 2020-2021 (6 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Arrays | Sorting | Sort both arrays + two-pointer counting | Easy |
| T2 | Path in Graph | Graph | DFS connected component labeling, O(1) per query | Medium |
| T3 | Reverse Linked List Subrange | Linked List | In-place partial reversal with pointer rewiring | Hard |
| T4 | Subarray (Distinct) | Sliding Window | Sliding window + hash map for longest distinct subarray | Medium |
| T5 | Support Center | Priority Queue | Multiset batch min-extraction simulation | Easy |
| T6 | Cycles in Graph | MST / Union-Find | **Maximum** spanning tree (Kruskal reversed), answer = total - MST | Medium |

### 2021-2022 (5 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Unique Rows | Hash Map | `map<vector, int>` frequency counting | Easy |
| T2 | SDA Exam | Dijkstra | Shortest path in weighted graph (all edges weight 6) | Easy |
| T3 | Linked Numbers | Linked List + Stack | Digit-by-digit addition, stack to reverse output | Medium |
| T4 | Pancakes | Binary Search | Binary search on the answer (min time for K chefs) | Medium |
| T5 | Leafs | DAG + DFS | Post-order DFS with memoization on DAG | Hard |

### 2022-2023 (5 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Brotherly Beating | Hash Map | Frequency map simulation (sticker collection) | Easy |
| T2 | Facebook Friends | Graph / DFS | Connected components, count those with size % K == 0 | Easy |
| T3 | Two PQs | Priority Queue | Two min-heaps: events by time + free chairs | Medium |
| T4 | Shortest Path Sequence | BFS | Sequential BFS with forbidden + future-waypoint blocking | Medium |
| T5 | Best-Selling Item | Binary Search + Hash Map | Precompute best-seller at each event, `lower_bound` queries | Hard |

### 2023-2024 (5 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Strings and Counting | Hash Map | Frequency-of-frequencies analysis (can remove 1 char?) | Easy |
| T2 | Closest Element | Binary Search | `upper_bound` on descending array, return left neighbor | Medium |
| T3 | Cycles in Components | Graph / DFS | DFS cycle detection (parent tracking), count cyclic components | Medium |
| T4 | Many MSTs | MST + Union-Find | Kruskal's + DSU, sum MST weights for components with size % K == 0 | Hard |
| T5 | Coloring a Tree | Tree / Greedy | Post-order DFS, greedy bottom-up paint propagation | Medium |

### 2024-2025 (5 tasks)

| Task | Name | Topic | Algorithm | Difficulty |
|------|------|-------|-----------|------------|
| T1 | Custom Sorting | Sorting | Sort by A^2/B ratio descending, custom comparator | Easy |
| T2 | Is It a Tree? | Graph / DFS | DFS: check connected + no cycles | Medium |
| T3 | Subarray Sum = K | Prefix Sum + Hash Map | Prefix sum + defaultdict, count + longest subarray | Medium |
| T4 | K-Clustering | Union-Find / Kruskal | Kruskal stopped early at K components, track max edge | Medium-Hard |
| T5 | LCS | Dynamic Programming | 2D DP table for Longest Common Subsequence | Hard |

---

## 2. Task Slot Patterns

This is the most important section. Each task slot has a clear pattern.

### Task 1: Always EASY -- Hash Map, Sorting, or BST basics

| Year | Topic |
|------|-------|
| 2018-2019 | BST insert + preorder print |
| 2019-2020 | Graph edge lookup (adjacency map) |
| 2020-2021 | Sorting + two-pointer counting |
| 2021-2022 | Hash map (vector as key, count unique rows) |
| 2022-2023 | Hash map simulation (sticker frequency) |
| 2023-2024 | Hash map frequency-of-frequencies |
| 2024-2025 | Custom sorting with lambda |

**What to expect:** A warm-up problem. Usually hash map frequency counting or sorting. Should take 10-15 minutes. **Free points.**

**Must know:**
- `unordered_map<key, int>` frequency counting
- `sort()` with custom comparator / lambda
- Basic BST insert (unlikely now, but possible)

---

### Task 2: MEDIUM -- Graph DFS/BFS OR Binary Search

| Year | Topic |
|------|-------|
| 2018-2019 | BST delete + BFS level-order (odd levels) |
| 2019-2020 | Binary search (find positions in sorted array) |
| 2020-2021 | DFS connected component labeling |
| 2021-2022 | Dijkstra shortest path |
| 2022-2023 | DFS connected components (size % K) |
| 2023-2024 | Binary search (`upper_bound`, closest element) |
| 2024-2025 | DFS (check if graph is tree: connected + acyclic) |

**Pattern:** Alternates between **graph traversal** (4/7 years) and **binary search** (2/7 years).

**Must know:**
- DFS connected component counting (with size tracking)
- BFS level-order traversal
- `lower_bound` / `upper_bound` on sorted arrays
- Cycle detection in undirected graphs (DFS + parent)
- "Is it a tree?" check: connected + no cycles

---

### Task 3: MEDIUM -- Most varied slot

| Year | Topic |
|------|-------|
| 2018-2019 | Hash map (word frequency, filter, sort) |
| 2019-2020 | DFS connected components |
| 2020-2021 | Linked list partial reversal |
| 2021-2022 | Linked list digit addition + stack |
| 2022-2023 | Priority queue (two heaps simulation) |
| 2023-2024 | DFS cycle detection in components |
| 2024-2025 | Prefix sum + hash map (subarray sum = K) |

**Pattern:** This slot has the most variety. In recent years (2022+): graph DFS or prefix sums + hash maps.

**Must know:**
- Linked list manipulation (reversal, digit addition) -- less likely now
- Priority queue simulation
- Prefix sum + hash map for subarray problems
- DFS with cycle detection

---

### Task 4: MEDIUM-HARD -- Graph algorithms (MST/BFS) OR Binary Search on Answer

| Year | Topic |
|------|-------|
| 2018-2019 | Directed graph cycle detection (DFS) |
| 2019-2020 | BST left-view (BFS level-order) |
| 2020-2021 | Sliding window (longest subarray) |
| 2021-2022 | Binary search on the answer (min time for chefs) |
| 2022-2023 | BFS shortest path with forbidden nodes + waypoints |
| 2023-2024 | **Kruskal's MST + DSU** (components with size % K) |
| 2024-2025 | **K-Clustering (DSU + Kruskal stopped early)** |

**Pattern:** Last 2 years are consistently **MST + Union-Find**. Before that it varied. This is the slot where DSU matters most.

**Must know:**
- **Kruskal's MST + Union-Find** (appeared 2 consecutive years, very likely again)
- BFS with blocked/forbidden nodes
- Binary search on the answer
- Sliding window

---

### Task 5: HARD -- The differentiator

| Year | Topic |
|------|-------|
| 2019-2020 | BFS shortest path with ordered waypoints |
| 2020-2021 | Max spanning tree (Kruskal reversed) -- remove min weight to make acyclic |
| 2021-2022 | DAG post-order DFS with memoization |
| 2022-2023 | Binary search + hash map (timestamped queries, precompute + lower_bound) |
| 2023-2024 | Tree greedy (post-order DFS, min dominating set / painting) |
| 2024-2025 | Dynamic Programming (LCS, 2D DP) |

**Pattern:** This is the hardest task. Tests advanced understanding. Recent trend: **Tree DFS greedy** or **DP**.

**Must know:**
- Post-order DFS on trees with state propagation (greedy painting/cameras)
- Dynamic Programming (LCS, grid DP, coin change)
- BFS with complex constraints
- Precomputation + binary search for offline queries

---

## 3. Topic Frequency Across All 7 Years

| Topic | Years it appeared (task #) | Total | PRIORITY |
|-------|---------------------------|-------|----------|
| **BFS/DFS on graphs** | 18(T4), 19(T3,T5), 20(T2), 22(T2,T4), 23(T3), 24(T2) | **6/7 years** | CRITICAL |
| **Hash Map / frequency** | 18(T3), 21(T1), 22(T1), 23(T1), 24(T3) | **5/7 years** | CRITICAL |
| **Binary Search** | 19(T2), 21(T4), 22(T5), 23(T2) | **4/7 years** | HIGH |
| **BST / Tree operations** | 18(T1,T2), 19(T4), 23(T5) | **3/7 years** | HIGH |
| **MST + Union-Find (Kruskal)** | 20(T6), 23(T4), 24(T4) | **3/7 years** | HIGH |
| **Priority Queue / Heap** | 20(T5), 21(T2), 22(T3) | **3/7 years** | HIGH |
| **Linked Lists** | 20(T3), 21(T3) | **2/7 years** | MEDIUM |
| **Sorting** | 20(T1), 24(T1) | **2/7 years** | MEDIUM |
| **Cycle detection** | 18(T4), 23(T3) | **2/7 years** | MEDIUM |
| **BFS with waypoints/forbidden** | 19(T5), 22(T4) | **2/7 years** | MEDIUM |
| **Sliding Window** | 20(T4) | **1/7 years** | LOW |
| **Dijkstra** | 21(T2) | **1/7 years** | MEDIUM |
| **DAG + memoization** | 21(T5) | **1/7 years** | LOW |
| **Prefix Sum + Hash Map** | 24(T3) | **1/7 years** | MEDIUM |
| **Dynamic Programming** | 24(T5) | **1/7 years** | MEDIUM |
| **Tree greedy (post-order)** | 23(T5) | **1/7 years** | MEDIUM |

### The pattern is clear:

1. **BFS/DFS** appears EVERY year (or nearly every year). It's the backbone of the exam.
2. **Hash Maps** appear most years, always as an easy task.
3. **Binary Search** appears every other year.
4. **MST + Union-Find** has appeared the last 3 exams that included hard graph tasks. Trend is increasing.
5. **Trees/BST** used to be common (2018-2019) but declining. Still possible.
6. **DP** only appeared once (2024-2025) but is a course topic, so it can appear again.

---

## 4. Detailed Exam Breakdown by Year

### 2018-2019

**T1 -- Tree Operations Part 1 (BST Insert + Pre-order Print)**
- Implement `add(int X)` for a BST (ignore duplicates) and `print()` for pre-order traversal.
- **Approach:** Iterative BST insertion, recursive pre-order (root, left, right).

**T2 -- Tree Operations Part 2 (BST Delete + Print Odd Levels)**
- Implement `remove(int X)` with all 3 cases (leaf, one child, two children using in-order successor) and `print_odd_layers()` (print levels 1, 3, 5...).
- **Approach:** BST deletion with successor replacement. BFS level-order with level counter, print only when `level % 2 == 1`.

**T3 -- Dundee The Crocodile (Unique Words)**
- Given two sentences, find words that appear exactly once total across both sentences. Output sorted.
- **Approach:** `unordered_map<string, int>` counts all words. Filter for count == 1. Sort and print.

**T4 -- Cyclic Graph (Directed Cycle Detection)**
- Given T directed weighted graphs, determine if each contains a cycle. Output true/false.
- **Approach:** DFS with recursion stack tracking (`set<int>`). If a neighbor is already in the current recursion path, cycle found. Backtrack by removing from set.

---

### 2019-2020

**T1 -- Road Check (Path Validation)**
- Given a weighted undirected graph and a sequence of vertices, check if the sequence forms a valid path. If yes, output total weight. If no, output -1.
- **Approach:** Store graph as `unordered_map<int, unordered_map<int, int>>`. For each consecutive pair, check if edge exists and accumulate weight.

**T2 -- Find Element (Binary Search Positions)**
- Given a sorted array and Q queries, for each query: if element exists, output first and last index. If not, output insertion position.
- **Approach:** Preprocess first/last positions with hash map. Use `upper_bound` for insertion point. O(log N) per query.

**T3 -- Counting Regions (Connected Components)**
- Given T undirected graphs, count connected components for each.
- **Approach:** DFS from each unvisited vertex. Count how many DFS launches = number of components.

**T4 -- Tree Profile (BST Left View)**
- Build a BST from N insertions, then print the left-side view (leftmost node at each level).
- **Approach:** BST insert + BFS level-order. At each level, print the first node dequeued.

**T5 -- Shortest Tour (BFS with Ordered Waypoints)**
- Given a directed unweighted graph and K waypoints to visit in order, find shortest total path.
- **Approach:** For each consecutive waypoint pair, run BFS. Block future waypoints during BFS so they can't be visited prematurely. Sum all segment distances.

---

### 2020-2021

**T1 -- Arrays (Count Smaller Elements)**
- For each element in array Y, count how many elements in array X are strictly smaller.
- **Approach:** Sort both arrays. Two-pointer scan. Store results in hash map keyed by value.

**T2 -- Path in Graph (Connected Component Queries)**
- Given an undirected graph and K vertex pairs, for each pair answer: connected (1) or not (0)?
- **Approach:** DFS to label each vertex with its component ID. Query is O(1): compare labels.

**T3 -- Reverse Linked List Subrange**
- Reverse the sub-list from position n to m in a singly linked list.
- **Approach:** Walk to position n-1, then reverse pointers from n to m. Reconnect the boundary nodes.

**T4 -- Subarray (Longest Distinct)**
- Find the longest contiguous subarray with all unique elements.
- **Approach:** Sliding window with `unordered_map<int, int>` tracking last-seen index. When duplicate found, move left boundary past the previous occurrence.

**T5 -- Support Center (Batch Min Extraction)**
- Process requests with priorities in batches of B every T time units, K batches total. Output selected requests.
- **Approach:** `multiset<int>` to always extract the smallest. Pop B elements from `begin()` every T steps.

**T6 -- Cycles in Graph (Min Weight Removal)**
- Given an undirected weighted graph, find minimum total edge weight to remove to make it acyclic.
- **Approach:** Build a **Maximum** Spanning Tree using Kruskal's (sort edges descending). Answer = total_weight - max_spanning_tree_weight. Uses Union-Find.

---

### 2021-2022

**T1 -- Unique Rows**
- Count rows that appear exactly once in a matrix.
- **Approach:** Use `map<vector<int>, int>` to count row frequencies. Count entries with value == 1.

**T2 -- SDA Exam (Shortest Path)**
- Find shortest distances from Ivancho to all other students in a graph with uniform edge weight 6.
- **Approach:** Dijkstra with priority queue (BFS would also work since uniform weights).

**T3 -- Linked Numbers (Add Two Numbers in Linked Lists)**
- Two numbers stored as reversed linked lists. Output their sum.
- **Approach:** Traverse both lists simultaneously, add digits with carry. Push results onto a stack, then pop to print in correct order.

**T4 -- Pancakes (Min Time for K Chefs)**
- K chefs with different speeds must make N pancakes. Find minimum time.
- **Approach:** Binary search on the answer. For candidate time T, check: sum of (T / speed_i) >= N? If yes, try smaller. If no, try larger.

**T5 -- Leafs (DAG Descendant Sum)**
- Given a DAG and queries, for each query vertex output the sum of all descendant weights.
- **Approach:** Post-order DFS with memoization. `sums[node] = sum of (child + sums[child])` for all children.

---

### 2022-2023

**T1 -- Brotherly Beating (Sticker Simulation)**
- Over N days, receive one sticker and one demand per day. Count days when the demanded sticker isn't available.
- **Approach:** `unordered_map<int, int>` tracks sticker inventory. O(1) per day.

**T2 -- Facebook Friends (Components Divisible by K)**
- Count connected components whose size is divisible by K.
- **Approach:** DFS to find each component's size. Check `size % K == 0`.

**T3 -- Two PQs (Chair Assignment)**
- N people arrive/leave at different times. Each gets the lowest-numbered free chair. Find which chair person T sits in.
- **Approach:** Two priority queues: one for events sorted by time, one for free chairs (min-heap of chair numbers). Process events in order.

**T4 -- Shortest Path Sequence (BFS with Forbidden)**
- Visit P waypoints in order in an unweighted graph. K vertices are forbidden. Future waypoints also blocked.
- **Approach:** Sequential BFS between consecutive waypoints. Block forbidden vertices and not-yet-reached waypoints.

**T5 -- Best-Selling Item (Timestamped Queries)**
- N purchases (item_id, timestamp), then T queries asking "most sold item by time Q?"
- **Approach:** Process purchases chronologically, maintain running best-seller. Store snapshot at each timestamp. Answer queries with `lower_bound` binary search on timestamps.

---

### 2023-2024

**T1 -- Strings and Counting (Equal Frequencies After Removal)**
- Can removing exactly 1 character make all remaining character frequencies equal?
- **Approach:** Build char frequency map, then frequency-of-frequencies map. Case analysis on the distinct frequency values.

**T2 -- Closest Element (Binary Search in Descending Array)**
- Array sorted descending. For each query, find closest element, output the element to its left.
- **Approach:** Reverse to ascending, use `upper_bound`, compare candidates, translate back.

**T3 -- Cycles in Components**
- Count connected components that contain a cycle.
- **Approach:** DFS with parent tracking. If a visited non-parent neighbor is found, the component has a cycle.

**T4 -- Many MSTs (Kruskal + Component Filtering)**
- Compute MST of each connected component. Sum MST weights only for components with vertex count divisible by K.
- **Approach:** Kruskal's algorithm with Union-Find. After building MST forest, DFS to get component sizes and edge sums. Filter by K.

**T5 -- Coloring a Tree (Greedy Post-order)**
- Paint a binary tree: each paint bucket colors a node + its parent + its children. Find minimum buckets.
- **Approach:** Post-order DFS. If a node is uncovered, paint it and propagate coverage to parent/children/siblings.

---

### 2024-2025

**T1 -- Custom Sorting**
- Sort items by A^2/B ratio descending. Tiebreak by A descending.
- **Approach:** `sort()` with custom lambda comparator.

**T2 -- Is It a Tree?**
- Given T test cases with undirected graphs, check if each is a valid tree.
- **Approach:** DFS from vertex 0 with parent tracking. If cycle found OR not all vertices visited, it's not a tree.

**T3 -- Subarray Sum = K**
- Count subarrays with sum K and find the longest one.
- **Approach:** Prefix sum + hash map. Track count and earliest index for each prefix sum.

**T4 -- K-Clustering**
- Partition graph into K clusters using Kruskal's approach (stop when K components remain). Report max edge weight per cluster.
- **Approach:** Sort edges by weight, Union-Find, stop at K components. Track max edge per component.

**T5 -- Longest Common Subsequence**
- Find LCS of two strings.
- **Approach:** Classic 2D DP. `dp[i][j] = dp[i-1][j-1]+1` if match, else `max(dp[i-1][j], dp[i][j-1])`.

---

## 5. How to Think When You See a Task

### Step 1: Identify the input type

| Input | Think about... |
|-------|---------------|
| Graph (N vertices, M edges) | BFS, DFS, Union-Find, MST, Dijkstra |
| Sorted array + queries | Binary search (`lower_bound` / `upper_bound`) |
| Array/sequence | Sorting, sliding window, prefix sum, DP |
| Tree (N nodes, left/right children) | DFS (pre/in/post-order), BFS level-order |
| Strings | Hash map frequency, DP (LCS) |
| Multiple test cases (`T`) | Solve independently, clear state between cases |

### Step 2: What are they asking?

| Question type | Algorithm |
|--------------|-----------|
| "Count connected components" | DFS/BFS or Union-Find |
| "Components with size % K == 0" | DFS with size tracking |
| "Is it a tree?" | DFS: connected + no cycles |
| "Does it have a cycle?" (undirected) | DFS + parent tracking, or Union-Find |
| "Does it have a cycle?" (directed) | DFS with gray/black coloring |
| "Shortest path (unweighted)" | BFS |
| "Shortest path (weighted)" | Dijkstra |
| "Min cost to connect all" | MST (Kruskal or Prim) |
| "Split into K groups" | Kruskal stopped early |
| "Count subarrays with sum = K" | Prefix sum + hash map |
| "Longest subarray with property" | Sliding window |
| "K-th smallest/largest" | Heap / priority queue |
| "Minimum X such that condition" | Binary search on the answer |
| "Find position in sorted array" | `lower_bound` / `upper_bound` |
| "Longest common subsequence" | 2D DP |
| "Min coins / min steps" | 1D DP |
| "Min paint/cameras to cover tree" | Post-order DFS greedy |

### Step 3: Check constraints for acceptable complexity

| N size | OK complexity | Typical approach |
|--------|--------------|-----------------|
| <= 20 | O(2^N) | Brute force, backtracking |
| <= 1,000 | O(N^2) | Nested loops, 2D DP |
| <= 100,000 | O(N log N) | Sorting, binary search, heap |
| <= 1,000,000 | O(N) | Hash map, prefix sum, Union-Find |
| <= 10,000,000 | O(N) | Must be linear, fast I/O critical |

---

## 6. What to Expect in 2026

Based on 7 years of data:

| Task | Most likely topic | Confidence | Backup topic |
|------|------------------|------------|-------------|
| **T1** | Hash map frequency counting | 90% | Sorting with custom comparator |
| **T2** | DFS on graphs (components, tree check, cycles) | 70% | Binary search on sorted array |
| **T3** | Prefix sum + hash map, OR priority queue | 60% | DFS cycle detection, linked list |
| **T4** | **Kruskal MST + Union-Find** (3 years in a row!) | 75% | BFS with constraints, binary search on answer |
| **T5** | DP (LCS, grid, coin change) OR tree greedy DFS | 60% | DAG + memoization, complex BFS |

### Near-guaranteed topics (prepare these FIRST):
1. Hash map frequency counting (T1)
2. DFS on graphs: connected components, cycle detection, tree check (T2/T3)
3. Kruskal's MST + Union-Find (T4)

### Very likely topics (prepare these SECOND):
4. Binary search (`lower_bound`, `upper_bound`, search on answer)
5. BFS (shortest path, with forbidden nodes)
6. Dynamic Programming (LCS, grid DP, coin change)

### Possible topics (prepare if time allows):
7. Priority queue simulation
8. Post-order tree DFS with greedy state propagation
9. Prefix sum + hash map (subarray sum = K)
10. Sliding window

---

## 7. Priority Study Order

### Tier 1: MUST KNOW (covers T1, T2, T4 reliably)

**1. Hash Map frequency counting**
- `unordered_map<key, int>` for counting
- Frequency-of-frequencies pattern
- Filter by count, group by key
- **Practice:** Dundee The Crocodile (18-19 T3), Brotherly Beating (22-23 T1), Strings and Counting (23-24 T1)

**2. DFS on graphs**
- Connected component counting + size tracking
- Cycle detection in undirected graphs (parent tracking)
- Cycle detection in directed graphs (coloring: white/gray/black)
- "Is it a tree?" = connected + no cycles
- **Practice:** Counting Regions (19-20 T3), Facebook Friends (22-23 T2), Cycles in Components (23-24 T3), Is It a Tree (24-25 T2)

**3. BFS on graphs**
- Shortest path in unweighted graphs
- Level-order traversal (process level by level: `size = q.size()`)
- BFS with forbidden/blocked nodes
- **Practice:** Shortest Tour (19-20 T5), Shortest Path Sequence (22-23 T4)

**4. Kruskal's MST + Union-Find**
- Sort edges by weight, unite with DSU, skip if same component
- Track component size, max edge weight, or MST weight per component
- K-clustering variant: stop when K components remain
- **Practice:** Cycles in Graph (20-21 T6), Many MSTs (23-24 T4), K-Clustering (24-25 T4)

### Tier 2: HIGH PRIORITY (covers T2/T3/T5 alternatives)

**5. Binary Search**
- `lower_bound` / `upper_bound` on sorted arrays
- Binary search on the answer ("minimum X such that...")
- Finding closest element, first/last position
- **Practice:** Find Element (19-20 T2), Pancakes (21-22 T4), Closest Element (23-24 T2)

**6. Dynamic Programming**
- LCS (2D table): `dp[i][j] = dp[i-1][j-1]+1` if match, else `max(dp[i-1][j], dp[i][j-1])`
- Coin Change (1D): `dp[i] = min(dp[i], dp[i-coin]+1)`
- Grid DP: `dp[i][j] = min/max(dp[i-1][j], dp[i][j-1]) + grid[i][j]`
- House Robber: `dp[i] = max(dp[i-1], dp[i-2] + val[i])`
- **Practice:** LCS (24-25 T5)

**7. Priority Queue / Heap**
- Min-heap: `priority_queue<int, vector<int>, greater<int>>`
- Two-heap pattern (events + resources)
- Batch extraction with `multiset`
- **Practice:** Two PQs (22-23 T3), Support Center (20-21 T5)

### Tier 3: GOOD TO KNOW

**8. Tree DFS (post-order with state)**
- Process children first, then decide current node's state
- Greedy propagation (painting/cameras: 0=needs coverage, 1=is painter, 2=covered)
- **Practice:** Coloring a Tree (23-24 T5)

**9. Prefix Sum + Hash Map**
- Count subarrays with sum = K
- Longest subarray with sum = K
- **Practice:** Subarray Sum = K (24-25 T3)

**10. Sorting with custom comparator**
- Lambda: `sort(v.begin(), v.end(), [](auto& a, auto& b) { ... });`
- Sort indices: `iota` + sort by computed key
- **Practice:** Custom Sorting (24-25 T1), Arrays (20-21 T1)

**11. Sliding Window**
- Longest subarray with all distinct elements
- Longest subarray where each element appears at most K times
- **Practice:** Subarray (20-21 T4)

**12. Dijkstra**
- Shortest path with non-negative weights
- `priority_queue<pair<long long, int>, ..., greater<>>` + dist array
- **Practice:** SDA Exam (21-22 T2)

### Tier 4: UNLIKELY BUT POSSIBLE

**13. BST operations** (insert, delete, traversals) -- was big in 2018-2019, rarely since
**14. Linked list manipulation** -- appeared in 2020-2021 and 2021-2022, not since
**15. DAG + memoization** -- appeared once in 2021-2022
**16. Topological sort** -- never appeared on finals directly (covered in course though)

---

## Key Insight: The "Meta-Pattern"

Looking at all 7 years, the exam follows this structure:

```
T1: EASY warmup        → Hash map OR sorting         → 10-15 min
T2: MEDIUM standard    → Graph DFS/BFS OR bin search  → 20-30 min
T3: MEDIUM varied      → Mix of topics                → 25-35 min
T4: MEDIUM-HARD graphs → MST/Kruskal OR BFS complex   → 30-40 min
T5: HARD challenge     → DP OR tree greedy OR complex  → 40-60 min
```

**Strategy:** Solve T1 and T2 first (easiest, most predictable). Then attempt T4 if you know MST/DSU. Then T3. Leave T5 for last unless it's a DP you recognize.

**Time management (assuming 3-hour exam):**
- T1: 15 min
- T2: 25 min
- T3: 35 min
- T4: 40 min
- T5: 50 min
- Buffer: 15 min for debugging

**If you only have time to learn 3 things, learn these:**
1. DFS on graphs (connected components, cycles, tree check)
2. Hash map frequency counting
3. Kruskal's MST + Union-Find

These three cover T1, T2, and T4 in most years -- that's 3/5 tasks.
