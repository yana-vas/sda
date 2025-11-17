# 🎯 Complete Tree Problems Exam Guide

**Based on:** Past exams + "Expect 2 tree problems, one with traversal (stack/queue)"

---

## 📚 Table of Contents

1. [Exam Format & Strategy](#exam-format)
2. [Problem Categories](#categories)
3. [Essential Templates (MUST MEMORIZE)](#templates)
4. [Past Exam Problems (HIGH PRIORITY)](#past-exams)
5. [Common Traversal Problems](#traversals)
6. [BST-Specific Problems](#bst-problems)
7. [Edge Cases & Common Mistakes](#mistakes)
8. [Pre-Exam Checklist](#checklist)
9. [Quick Reference Cheat Sheet](#cheat-sheet)

---

## 🎓 Exam Format & Strategy <a name="exam-format"></a>

### What to Expect:

```
Problem 1: Traversal Problem (Stack/Queue Required)
- Level order traversal
- Right side view
- Iterative inorder/preorder/postorder
- Min depth (BFS early stopping)
- Symmetric tree check

Problem 2: General Tree/Graph Problem
- Subtree size calculation (PAST EXAM ⭐)
- Ancestor queries (PAST EXAM ⭐)
- Friend dependencies (PAST EXAM ⭐)
- Validate BST
- Range sum BST
- LCA (Lowest Common Ancestor)
```

### Time Management:

```
⏰ Total: 55 minutes for 2 problems

Problem 1 (Traversal): 20-25 minutes
- 5 min: understand problem + choose template
- 15 min: implement
- 5 min: test & debug

Problem 2 (General): 25-30 minutes
- 5 min: understand + plan approach
- 20 min: implement
- 5 min: test

Buffer: 5 minutes for edge cases
```

---

## 📊 Problem Categories <a name="categories"></a>

### Category A: Traversal (Stack/Queue) - 50% Probability

| Problem | Data Structure | Difficulty |
|---------|---------------|------------|
| Level Order Traversal | Queue (BFS) | ⭐⭐ |
| Right Side View | Queue (BFS) | ⭐⭐ |
| Inorder Iterative | Stack (DFS) | ⭐⭐⭐ |
| Min Depth | Queue (BFS) | ⭐⭐ |
| Symmetric Tree | Queue or Recursion | ⭐⭐ |

### Category B: Past Exam Problems - 40% Probability

| Problem | Technique | Difficulty |
|---------|-----------|------------|
| Subtree Size ⭐ | DFS Post-order | ⭐⭐⭐ |
| Ancestor Queries ⭐ | DFS Timestamps | ⭐⭐⭐⭐ |
| Friend Dependencies ⭐ | Sequential Set | ⭐⭐ |

### Category C: Classic BST - 10% Probability

| Problem | Technique | Difficulty |
|---------|-----------|------------|
| Validate BST | DFS Range Check | ⭐⭐⭐ |
| Range Sum BST | DFS Pruning | ⭐⭐ |
| LCA | DFS or BST Property | ⭐⭐⭐ |

---

## 🔥 Essential Templates (MUST MEMORIZE) <a name="templates"></a>

### Template 1: BFS Level Order ⭐⭐⭐⭐⭐

**USE FOR:** Level order, right side view, min depth, zigzag, width problems

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int levelSize = q.size();  // ⚠️ CRITICAL: capture BEFORE loop
        vector<int> currentLevel;
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            
            currentLevel.push_back(node->val);
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        
        result.push_back(currentLevel);
    }
    
    return result;
}
```

**Common Variations:**

```cpp
// Right Side View - take last element of each level
if (i == levelSize - 1) {
    result.push_back(node->val);
}

// Min Depth - stop at first leaf
if (!node->left && !node->right) {
    return depth;
}

// Average of Levels - calculate average per level
double avg = (double)sum / levelSize;

// Zigzag - alternate direction
int index = leftToRight ? i : levelSize - 1 - i;
level[index] = node->val;
```

---

### Template 2: Inorder Iterative with Stack ⭐⭐⭐⭐⭐

**USE FOR:** Inorder traversal, BST validation, kth smallest, BST recovery

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> stk;
    TreeNode* curr = root;
    
    while (curr || !stk.empty()) {
        // Step 1: Go to leftmost node
        while (curr) {
            stk.push(curr);
            curr = curr->left;
        }
        
        // Step 2: Process current node
        curr = stk.top();
        stk.pop();
        result.push_back(curr->val);
        
        // Step 3: Go to right subtree
        curr = curr->right;
    }
    
    return result;
}
```

**Key Pattern to Remember:**
```
while (curr || !stk.empty()) {
    while (curr) { push + go left }
    curr = stk.top(); pop;
    process(curr);
    curr = curr->right;
}
```

---

### Template 3: Preorder Iterative with Stack ⭐⭐⭐⭐

**USE FOR:** Preorder traversal, tree cloning, serialization

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> stk;
    stk.push(root);
    
    while (!stk.empty()) {
        TreeNode* node = stk.top();
        stk.pop();
        
        result.push_back(node->val);  // Process immediately
        
        // ⚠️ Push right first (so left is processed first)
        if (node->right) stk.push(node->right);
        if (node->left) stk.push(node->left);
    }
    
    return result;
}
```

---

### Template 4: Subtree Size (DFS Post-order) ⭐⭐⭐⭐⭐

**USE FOR:** Subtree size, subtree sum, balanced tree check

**PAST EXAM PROBLEM - VERY LIKELY!**

```cpp
// For general tree (adjacency list)
class SubtreeSize {
public:
    vector<int> calculateSubtreeSizes(int n, vector<vector<int>>& adj) {
        vector<int> subtreeSize(n, 0);
        dfs(0, -1, adj, subtreeSize);
        return subtreeSize;
    }
    
private:
    int dfs(int node, int parent, vector<vector<int>>& adj, 
            vector<int>& subtreeSize) {
        int size = 1;  // Count current node
        
        for (int child : adj[node]) {
            if (child != parent) {  // ⚠️ Don't revisit parent
                size += dfs(child, node, adj, subtreeSize);
            }
        }
        
        subtreeSize[node] = size;
        return size;
    }
};

// For binary tree
int subtreeSize(TreeNode* root) {
    if (!root) return 0;
    
    int leftSize = subtreeSize(root->left);
    int rightSize = subtreeSize(root->right);
    
    return 1 + leftSize + rightSize;
}
```

**Key Points:**
- ✅ Post-order: process children first, then current
- ✅ Always check `child != parent` in general tree
- ✅ Size includes the node itself (start with 1)

---

### Template 5: Ancestor Queries (DFS Timestamps) ⭐⭐⭐⭐⭐

**USE FOR:** Ancestor checking, subtree membership

**PAST EXAM PROBLEM - VERY LIKELY!**

```cpp
class AncestorQueries {
private:
    vector<int> inTime, outTime;
    int timer = 0;
    
public:
    AncestorQueries(int n) : inTime(n), outTime(n) {}
    
    void buildTimestamps(int node, int parent, vector<vector<int>>& adj) {
        inTime[node] = timer++;
        
        for (int child : adj[node]) {
            if (child != parent) {
                buildTimestamps(child, node, adj);
            }
        }
        
        outTime[node] = timer++;
    }
    
    bool isAncestor(int u, int v) {
        // u is ancestor of v if u's interval contains v's interval
        return inTime[u] <= inTime[v] && outTime[u] >= outTime[v];
    }
};

// Usage:
int main() {
    int n = 5;
    vector<vector<int>> adj(n);
    // build adjacency list...
    
    AncestorQueries aq(n);
    aq.buildTimestamps(0, -1, adj);
    
    // Query
    bool result = aq.isAncestor(0, 3);  // Is 0 ancestor of 3?
}
```

**How It Works:**
```
Tree:       0
           / \
          1   2
         /
        3

Timestamps (inTime, outTime):
0: (0, 7)  ← enters first, exits last
1: (1, 4)
3: (2, 3)  ← enters after 1, exits before 1
2: (5, 6)

Check if 1 is ancestor of 3:
  inTime[1]=1 <= inTime[3]=2 ✅
  outTime[1]=4 >= outTime[3]=3 ✅
  → YES, 1 is ancestor of 3
```

**Key Insight:**
- Ancestor's interval **contains** descendant's interval
- `inTime[u] <= inTime[v] && outTime[u] >= outTime[v]`

---

### Template 6: Friend Dependencies (Sequential Processing) ⭐⭐⭐⭐

**USE FOR:** Sequential dependency problems

**PAST EXAM PROBLEM - LIKELY!**

```cpp
class FriendDependencies {
public:
    int countTeamMembers(vector<pair<int, int>>& dependencies) {
        set<int> team;
        team.insert(1);  // Starting member
        
        // Process rules sequentially
        for (auto [x, y] : dependencies) {
            // ⚠️ Check if x is in team AT THIS MOMENT
            if (team.count(x)) {
                team.insert(y);
            }
            // If x not in team, y doesn't join (rule skipped)
        }
        
        return team.size();
    }
};

// Example:
// Input: [(1, 2), (2, 3), (4, 5), (3, 4)]
// Step 1: team = {1}, check 1→2: 1 in team, add 2 → {1, 2}
// Step 2: team = {1,2}, check 2→3: 2 in team, add 3 → {1, 2, 3}
// Step 3: team = {1,2,3}, check 4→5: 4 NOT in team, skip
// Step 4: team = {1,2,3}, check 3→4: 3 in team, add 4 → {1, 2, 3, 4}
// Result: 4
```

**Key Points:**
- ✅ Process rules **in order**
- ✅ Only add if prerequisite is **already** in team
- ✅ Use `set` for O(log n) lookup and automatic deduplication

---

### Template 7: Validate BST ⭐⭐⭐⭐

**USE FOR:** BST validation

```cpp
// Method 1: Range Checking (RECOMMENDED)
class ValidateBST {
public:
    bool isValidBST(TreeNode* root) {
        return validate(root, LONG_MIN, LONG_MAX);
    }
    
private:
    bool validate(TreeNode* node, long minVal, long maxVal) {
        if (!node) return true;
        
        // ⚠️ Use < and >, not <= and >=
        if (node->val <= minVal || node->val >= maxVal) {
            return false;
        }
        
        // Left: all values must be < node->val
        // Right: all values must be > node->val
        return validate(node->left, minVal, node->val) &&
               validate(node->right, node->val, maxVal);
    }
};

// Method 2: Inorder Traversal (Alternative)
class ValidateBST2 {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* prev = nullptr;
        return inorder(root, prev);
    }
    
private:
    bool inorder(TreeNode* node, TreeNode*& prev) {
        if (!node) return true;
        
        if (!inorder(node->left, prev)) return false;
        
        // ⚠️ Check: prev < current (strictly increasing)
        if (prev && prev->val >= node->val) return false;
        prev = node;
        
        return inorder(node->right, prev);
    }
};
```

---

### Template 8: Max/Min Depth ⭐⭐⭐

**USE FOR:** Tree depth problems

```cpp
// Max Depth - Simple DFS (one-liner)
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}

// Min Depth - BFS (RECOMMENDED - stops early)
int minDepth(TreeNode* root) {
    if (!root) return 0;
    
    queue<TreeNode*> q;
    q.push(root);
    int depth = 1;
    
    while (!q.empty()) {
        int size = q.size();
        
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            
            // ⚠️ First leaf found - return immediately
            if (!node->left && !node->right) {
                return depth;
            }
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        depth++;
    }
    
    return depth;
}

// Min Depth - DFS (careful with one child!)
int minDepth(TreeNode* root) {
    if (!root) return 0;
    
    // ⚠️ Leaf node
    if (!root->left && !root->right) return 1;
    
    // ⚠️ Only one child - must go to that child
    if (!root->left) return 1 + minDepth(root->right);
    if (!root->right) return 1 + minDepth(root->left);
    
    // Both children - take minimum
    return 1 + min(minDepth(root->left), minDepth(root->right));
}
```

---

### Template 9: Symmetric Tree ⭐⭐⭐

**USE FOR:** Mirror checking

```cpp
class SymmetricTree {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isMirror(root->left, root->right);
    }
    
private:
    bool isMirror(TreeNode* left, TreeNode* right) {
        // Both null
        if (!left && !right) return true;
        
        // One null, one not
        if (!left || !right) return false;
        
        // Check value and recurse
        return (left->val == right->val) &&
               isMirror(left->left, right->right) &&    // outer pair
               isMirror(left->right, right->left);      // inner pair
    }
};
```

---

### Template 10: LCA (Lowest Common Ancestor) ⭐⭐⭐

**USE FOR:** Finding common ancestors

```cpp
// For Binary Tree
class LCA {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Base case
        if (!root || root == p || root == q) {
            return root;
        }
        
        // Search in subtrees
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        
        // Both found in different subtrees
        if (left && right) return root;
        
        // Return whichever is not null
        return left ? left : right;
    }
};

// For BST (more efficient)
class LCA_BST {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Both smaller - go left
        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        }
        
        // Both larger - go right
        if (p->val > root->val && q->val > root->val) {
            return lowestCommonAncestor(root->right, p, q);
        }
        
        // Split point - this is LCA
        return root;
    }
};
```

---

## 🎯 Past Exam Problems (HIGH PRIORITY) <a name="past-exams"></a>

### Problem 1: Subtree Size ⭐⭐⭐⭐⭐

**COMPLETE SOLUTION:**

```cpp
#include <iostream>
#include <vector>
using namespace std;

class SubtreeSizeSolution {
public:
    vector<int> solve(int n, vector<pair<int, int>>& edges) {
        // Build adjacency list
        vector<vector<int>> adj(n);
        for (auto [u, v] : edges) {
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        
        // Calculate subtree sizes
        vector<int> subtreeSize(n, 0);
        dfs(0, -1, adj, subtreeSize);
        
        return subtreeSize;
    }
    
private:
    int dfs(int node, int parent, vector<vector<int>>& adj, 
            vector<int>& subtreeSize) {
        int size = 1;  // Count self
        
        for (int child : adj[node]) {
            if (child != parent) {  // Don't go back to parent
                size += dfs(child, node, adj, subtreeSize);
            }
        }
        
        subtreeSize[node] = size;
        return size;
    }
};

int main() {
    int n;
    cin >> n;
    
    vector<pair<int, int>> edges;
    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        edges.push_back({u, v});
    }
    
    SubtreeSizeSolution sol;
    vector<int> result = sol.solve(n, edges);
    
    for (int i = 0; i < n; i++) {
        cout << result[i];
        if (i < n - 1) cout << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Example:**
```
Input:
5
0 1
0 2
1 3
1 4

Tree:       0
           / \
          1   2
         / \
        3   4

Output: 5 3 1 1 1

Explanation:
Node 0: entire tree = 5 nodes
Node 1: nodes 1,3,4 = 3 nodes
Node 2: only itself = 1 node
Node 3: only itself = 1 node
Node 4: only itself = 1 node
```

---

### Problem 2: Ancestor Queries ⭐⭐⭐⭐⭐

**COMPLETE SOLUTION:**

```cpp
#include <iostream>
#include <vector>
using namespace std;

class AncestorSolution {
private:
    vector<int> inTime, outTime;
    int timer = 0;
    
public:
    void solve(int n, vector<pair<int, int>>& edges, 
               vector<pair<int, int>>& queries) {
        // Build adjacency list
        vector<vector<int>> adj(n);
        for (auto [u, v] : edges) {
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        
        // Resize timestamp arrays
        inTime.resize(n);
        outTime.resize(n);
        
        // Build timestamps
        dfs(0, -1, adj);
        
        // Answer queries
        for (auto [u, v] : queries) {
            if (isAncestor(u, v)) {
                cout << "YES\n";
            } else {
                cout << "NO\n";
            }
        }
    }
    
private:
    void dfs(int node, int parent, vector<vector<int>>& adj) {
        inTime[node] = timer++;
        
        for (int child : adj[node]) {
            if (child != parent) {
                dfs(child, node, adj);
            }
        }
        
        outTime[node] = timer++;
    }
    
    bool isAncestor(int u, int v) {
        return inTime[u] <= inTime[v] && outTime[u] >= outTime[v];
    }
};

int main() {
    int n, q;
    cin >> n >> q;
    
    vector<pair<int, int>> edges;
    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        edges.push_back({u, v});
    }
    
    vector<pair<int, int>> queries;
    for (int i = 0; i < q; i++) {
        int u, v;
        cin >> u >> v;
        queries.push_back({u, v});
    }
    
    AncestorSolution sol;
    sol.solve(n, edges, queries);
    
    return 0;
}
```

**Example:**
```
Input:
5 3
0 1
0 2
1 3
1 4
0 3
1 3
3 1

Tree:       0
           / \
          1   2
         / \
        3   4

Queries:
1. Is 0 ancestor of 3? → YES (0 → 1 → 3)
2. Is 1 ancestor of 3? → YES (1 → 3)
3. Is 3 ancestor of 1? → NO (3 is below 1)

Output:
YES
YES
NO
```

---

### Problem 3: Friend Dependencies ⭐⭐⭐⭐

**COMPLETE SOLUTION:**

```cpp
#include <iostream>
#include <vector>
#include <set>
using namespace std;

class FriendDependenciesSolution {
public:
    int solve(int n, vector<pair<int, int>>& rules) {
        set<int> team;
        team.insert(1);  // Starting member
        
        // Process rules sequentially
        for (auto [x, y] : rules) {
            if (team.count(x)) {
                team.insert(y);
            }
        }
        
        return team.size();
    }
};

int main() {
    int n;
    cin >> n;
    
    vector<pair<int, int>> rules;
    for (int i = 0; i < n; i++) {
        int x, y;
        cin >> x >> y;
        rules.push_back({x, y});
    }
    
    FriendDependenciesSolution sol;
    cout << sol.solve(n, rules) << "\n";
    
    return 0;
}
```

**Example:**
```
Input:
4
1 2
2 3
4 5
3 4

Processing:
Start: team = {1}
Rule 1→2: 1 in team? YES → add 2 → team = {1, 2}
Rule 2→3: 2 in team? YES → add 3 → team = {1, 2, 3}
Rule 4→5: 4 in team? NO → skip
Rule 3→4: 3 in team? YES → add 4 → team = {1, 2, 3, 4}

Output: 4
```

---

## ❌ Common Mistakes & How to Avoid Them <a name="mistakes"></a>

### Mistake 1: Not Capturing Level Size in BFS

```cpp
// ❌ WRONG
while (!q.empty()) {
    for (int i = 0; i < q.size(); i++) {  // q.size() changes!
        // ...
    }
}

// ✅ CORRECT
while (!q.empty()) {
    int size = q.size();  // Capture BEFORE loop
    for (int i = 0; i < size; i++) {
        // ...
    }
}
```

---

### Mistake 2: Forgetting `curr = curr->right` in Inorder

```cpp
// ❌ WRONG
while (curr || !stk.empty()) {
    while (curr) {
        stk.push(curr);
        curr = curr->left;
    }
    curr = stk.top(); stk.pop();
    result.push_back(curr->val);
    // Missing: curr = curr->right!
}

// ✅ CORRECT
while (curr || !stk.empty()) {
    while (curr) {
        stk.push(curr);
        curr = curr->left;
    }
    curr = stk.top(); stk.pop();
    result.push_back(curr->val);
    curr = curr->right;  // Don't forget!
}
```

---

### Mistake 3: Min Depth with One Child

```cpp
// ❌ WRONG - treats one child as leaf
int minDepth(TreeNode* root) {
    if (!root) return 0;
    if (!root->left || !root->right) return 1;  // WRONG!
    return 1 + min(minDepth(root->left), minDepth(root->right));
}

// ✅ CORRECT - handle one child case
int minDepth(TreeNode* root) {
    if (!root) return 0;
    if (!root->left && !root->right) return 1;  // Leaf
    
    if (!root->left) return 1 + minDepth(root->right);  // Only right
    if (!root->right) return 1 + minDepth(root->left);  // Only left
    
    return 1 + min(minDepth(root->left), minDepth(root->right));
}
```

---

### Mistake 4: BST Validation with `<=` Instead of `<`

```cpp
// ❌ WRONG
if (node->val < minVal || node->val > maxVal) {  // Allows duplicates!
    return false;
}

// ✅ CORRECT
if (node->val <= minVal || node->val >= maxVal) {  // Strict bounds
    return false;
}
```

---

### Mistake 5: Not Checking Parent in Tree DFS

```cpp
// ❌ WRONG - infinite loop!
void dfs(int node, vector<vector<int>>& adj) {
    for (int child : adj[node]) {
        dfs(child, adj);  // Will go back to parent!
    }
}

// ✅ CORRECT
void dfs(int node, int parent, vector<vector<int>>& adj) {
    for (int child : adj[node]) {
        if (child != parent) {  // Don't revisit parent
            dfs(child, node, adj);
        }
    }
}
```

---

### Mistake 6: Wrong Push Order in Preorder Stack

```cpp
// ❌ WRONG - processes right before left
if (node->left) stk.push(node->left);
if (node->right) stk.push(node->right);

// ✅ CORRECT - right first so left pops first
if (node->right) stk.push(node->right);
if (node->left) stk.push(node->left);
```

---

### Mistake 7: Using INT_MIN/MAX for BST Validation

```cpp
// ❌ WRONG - fails when node value is INT_MIN or INT_MAX
bool validate(TreeNode* node, int minVal, int maxVal) {
    // ...
}

// ✅ CORRECT - use LONG_MIN/LONG_MAX
bool validate(TreeNode* node, long minVal, long maxVal) {
    // ...
}
```

---

### Mistake 8: Forgetting to Initialize Adjacency List

```cpp
// ❌ WRONG
vector<vector<int>> adj;
for (auto [u, v] : edges) {
    adj[u].push_back(v);  // Out of bounds!
}

// ✅ CORRECT
int n = 5;
vector<vector<int>> adj(n);  // Pre-allocate!
for (auto [u, v] : edges) {
    adj[u].push_back(v);
}
```

---

### Mistake 9: Not Handling Null Root

```cpp
// ❌ WRONG - crashes on null
vector<int> levelOrder(TreeNode* root) {
    queue<TreeNode*> q;
    q.push(root);  // If root is null, queue has null!
    // ...
}

// ✅ CORRECT
vector<int> levelOrder(TreeNode* root) {
    if (!root) return {};  // Check first!
    queue<TreeNode*> q;
    q.push(root);
    // ...
}
```

---

### Mistake 10: Sequential vs. Graph Dependencies

```cpp
// Friend Dependencies: Process SEQUENTIALLY
for (auto [x, y] : rules) {
    if (team.count(x)) {  // Check at THIS moment
        team.insert(y);
    }
}

// Graph Dependencies: Process ALL at once (DFS/BFS)
// Different problem type!
```

---

## ✅ Pre-Exam Checklist <a name="checklist"></a>

### Day Before Exam:

**Must Write from Memory (5 min each):**
- [ ] BFS level order template
- [ ] Inorder iterative template
- [ ] Subtree size DFS template
- [ ] Ancestor queries (timestamps)
- [ ] Validate BST (range method)

**Must Understand:**
- [ ] When to use queue vs. stack
- [ ] How to capture level size in BFS
- [ ] Why we check `child != parent` in tree DFS
- [ ] Difference between max/min depth
- [ ] How timestamp intervals work for ancestors

**Edge Cases to Remember:**
- [ ] Null root
- [ ] Single node
- [ ] Skewed tree (linked list)
- [ ] One child (min depth)
- [ ] INT_MIN/MAX values (BST)

---

### 30 Minutes Before Exam:

**Quick Practice (Mental):**

1. **BFS**: Can you write the template with eyes closed?
2. **Inorder**: What are the 3 steps in the while loop?
3. **Subtree Size**: What do you return? What do you count?
4. **Ancestors**: What's the condition for u being ancestor of v?
5. **Edge Cases**: What if root is null? What if tree has 1 node?

**Mental Walkthrough:**
```
Problem appears → Identify category → Choose template → Write code → Test edge cases
```

---

## 📋 Quick Reference Cheat Sheet <a name="cheat-sheet"></a>

### BFS Template (Queue)
```cpp
queue<TreeNode*> q;
q.push(root);
while (!q.empty()) {
    int size = q.size();  // ⚠️ CRITICAL
    for (int i = 0; i < size; i++) {
        TreeNode* node = q.front(); q.pop();
        // process...
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}
```

### Inorder Iterative (Stack)
```cpp
stack<TreeNode*> stk;
TreeNode* curr = root;
while (curr || !stk.empty()) {
    while (curr) { stk.push(curr); curr = curr->left; }
    curr = stk.top(); stk.pop();
    // process curr->val
    curr = curr->right;
}
```

### Subtree Size (DFS)
```cpp
int dfs(int node, int parent, adj, subtreeSize) {
    int size = 1;
    for (int child : adj[node]) {
        if (child != parent) {
            size += dfs(child, node, adj, subtreeSize);
        }
    }
    return subtreeSize[node] = size;
}
```

### Ancestor Queries (Timestamps)
```cpp
void dfs(int node, int parent, adj) {
    inTime[node] = timer++;
    for (child : adj[node]) {
        if (child != parent) dfs(child, node, adj);
    }
    outTime[node] = timer++;
}

bool isAncestor(u, v) {
    return inTime[u] <= inTime[v] && outTime[u] >= outTime[v];
}
```

### Validate BST
```cpp
bool validate(node, minVal, maxVal) {
    if (!node) return true;
    if (node->val <= minVal || node->val >= maxVal) return false;
    return validate(left, minVal, node->val) && 
           validate(right, node->val, maxVal);
}
```

### Max/Min Depth
```cpp
// Max (DFS)
return !root ? 0 : 1 + max(maxDepth(left), maxDepth(right));

// Min (BFS - stops at first leaf)
while (!q.empty()) {
    // ... level loop
    if (!node->left && !node->right) return depth;
}
```

---

## 🎯 Problem Recognition Quick Guide

| You See | Use This |
|---------|----------|
| "level by level" | BFS + capture size |
| "each level" | BFS + capture size |
| "from right" / "rightmost" | BFS + take last in level |
| "iterative traversal" | Stack (inorder/preorder) |
| "subtree size" | DFS post-order + return size |
| "ancestor" + "queries" | DFS timestamps |
| "sequential" / "in order" | Process with set/map |
| "valid BST" | Range validation |
| "depth" / "height" | DFS (max) or BFS (min) |
| "mirror" / "symmetric" | Recursive check |

---

## ⏰ Exam Time Strategy

### Problem 1 (Traversal - 25 min):
```
0-5 min:   Read carefully, identify pattern
5-7 min:   Choose template (BFS/Stack)
7-20 min:  Implement
20-23 min: Test with example
23-25 min: Check edge cases (null, single node)
```

### Problem 2 (General - 30 min):
```
0-5 min:   Read, identify if it matches past exam
5-10 min:  Plan approach, draw example
10-25 min: Implement
25-28 min: Test
28-30 min: Edge cases + review
```

---

## 🚨 CRITICAL REMINDERS

### Always Write These First:
```cpp
// Check null
if (!root) return /* appropriate value */;

// BFS: capture size
int size = q.size();

// Tree DFS: check parent
if (child != parent)

// BST: use LONG_MIN/LONG_MAX
validate(root, LONG_MIN, LONG_MAX)
```

### Never Forget:
1. ✅ `curr = curr->right` in inorder
2. ✅ Capture `size` before BFS loop
3. ✅ `child != parent` in tree DFS
4. ✅ Leaf check: `!left && !right` (both null)
5. ✅ One child case in min depth

---

## 🎓 Final Wisdom

### Most Likely Scenarios:

**70% Probability:**
- Problem 1: Level Order / Right Side View / Min Depth (BFS)
- Problem 2: Subtree Size / Ancestor Queries (Past exam)

**20% Probability:**
- Problem 1: Iterative Traversal (Stack)
- Problem 2: Validate BST / LCA

**10% Probability:**
- Problem 1: Symmetric Tree
- Problem 2: Friend Dependencies

### What to Do If Stuck:

1. **Identify category first** - BFS? DFS? Past exam?
2. **Draw the example** - visualize the tree
3. **Pick the closest template** - adapt if needed
4. **Start with base cases** - null, single node
5. **Test as you go** - don't wait until the end

### Confidence Boosters:

- ✅ You know all the templates
- ✅ You've seen past exam problems
- ✅ You understand BFS vs. DFS
- ✅ You can handle edge cases
- ✅ **You got this!** 🚀

---

## 💪 You Are Ready!

**Remember:**
- 📝 Templates are your friends - use them!
- 🎯 Past exams are high priority - know them cold
- ⏰ Time management - don't get stuck
- 🧪 Test edge cases - null, single, skewed
- 💡 Stay calm - you've prepared well!

**Good luck on your exam!** 🍀

You've got all the tools you need. Trust your preparation! 🌟

---

## 📚 Additional Resources

### Tree Node Definition (for reference):
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

### Common Includes:
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <set>
#include <climits>
using namespace std;
```

---

**Created by: Claude (Anthropic)**  
**Last Updated: November 2024**  
**Good luck on your Data Structures exam!** 🎓
