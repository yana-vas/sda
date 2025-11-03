# 📚 ULTIMATE Linked List Exam Guide

## 🎯 Table of Contents
1. All Possible Operations (Singly & Doubly)
2. When to Use Singly vs Doubly
3. Universal Template (Always Write This)
4. Problem-Solving Framework
5. Quick Reference Schemes

---

# PART 1: ALL POSSIBLE OPERATIONS

## 📋 Complete Operations List

### ⭐ BASIC OPERATIONS (Most Common)

| Operation | Singly LL | Doubly LL | Frequency on Exams |
|-----------|-----------|-----------|-------------------|
| Insert at head | ⭐⭐⭐ | ⭐⭐⭐ | Very High |
| Insert at tail | ⭐⭐⭐ | ⭐⭐⭐ | Very High |
| Delete head | ⭐⭐⭐ | ⭐⭐⭐ | Very High |
| Delete tail | ⭐⭐ | ⭐⭐⭐ | High |
| Search/Find | ⭐⭐⭐ | ⭐⭐⭐ | Very High |
| Insert at position | ⭐⭐⭐ | ⭐⭐ | High |
| Delete at position | ⭐⭐⭐ | ⭐⭐ | High |
| Delete by value | ⭐⭐⭐ | ⭐⭐ | High |

### 🔄 MANIPULATION OPERATIONS

| Operation | Singly LL | Doubly LL | Frequency |
|-----------|-----------|-----------|-----------|
| Reverse list | ⭐⭐⭐ | ⭐⭐ | Very High |
| Find middle | ⭐⭐⭐ | ⭐⭐ | High |
| Detect cycle | ⭐⭐⭐ | ⭐ | Medium |
| Remove duplicates | ⭐⭐⭐ | ⭐⭐ | High |
| Merge two lists | ⭐⭐⭐ | ⭐⭐ | Medium |
| Split list | ⭐⭐ | ⭐ | Low |

### 🔍 QUERY OPERATIONS

| Operation | Frequency | Notes |
|-----------|-----------|-------|
| Get size/length | ⭐⭐⭐ | Easy |
| Check if empty | ⭐⭐⭐ | Easy |
| Get Nth node | ⭐⭐⭐ | Common |
| Get Nth from end | ⭐⭐ | Tricky |
| Check palindrome | ⭐⭐ | Medium |
| Find intersection | ⭐ | Hard |

---

# PART 2: OPERATION DETAILS & PATTERNS

## 1️⃣ INSERT AT HEAD

### Singly Linked:
```cpp
void insertAtHead(int val) {
    Node* newNode = new Node(val);
    newNode->next = head;  // Connect to rest
    head = newNode;        // Update head
}
```

**Pattern:**
```
Before: [A] → [B] → [C]
        ^head

Step 1: Create [X]
Step 2: X->next = head
Step 3: head = X

After:  [X] → [A] → [B] → [C]
        ^head
```

**Complexity:** O(1)
**Edge Cases:** Empty list

---

### Doubly Linked:
```cpp
void insertAtHead(int val) {
    Node* newNode = new Node(val);
    newNode->next = head;
    newNode->prev = nullptr;
    
    if (head) {
        head->prev = newNode;
    }
    head = newNode;
    
    if (!tail) {
        tail = newNode;  // First node
    }
}
```

**Extra steps:** Update prev pointers, check tail

---

## 2️⃣ INSERT AT TAIL

### Singly Linked:
```cpp
void insertAtTail(int val) {
    Node* newNode = new Node(val);
    
    if (!head) {
        head = newNode;
        return;
    }
    
    Node* curr = head;
    while (curr->next) {
        curr = curr->next;
    }
    curr->next = newNode;
}
```

**Complexity:** O(n) - must traverse

---

### Doubly Linked (with tail pointer):
```cpp
void insertAtTail(int val) {
    Node* newNode = new Node(val);
    
    if (!tail) {
        head = tail = newNode;
        return;
    }
    
    newNode->prev = tail;
    tail->next = newNode;
    tail = newNode;
}
```

**Complexity:** O(1) - have tail pointer!

---

## 3️⃣ DELETE HEAD

### Singly Linked:
```cpp
void deleteHead() {
    if (!head) return;
    
    Node* temp = head;
    head = head->next;
    delete temp;
}
```

**Pattern:** Save → Update → Delete

---

### Doubly Linked:
```cpp
void deleteHead() {
    if (!head) return;
    
    Node* temp = head;
    head = head->next;
    
    if (head) {
        head->prev = nullptr;
    } else {
        tail = nullptr;  // Was only node
    }
    
    delete temp;
}
```

**Extra:** Update prev, check tail

---

## 4️⃣ DELETE TAIL

### Singly Linked:
```cpp
void deleteTail() {
    if (!head) return;
    
    if (!head->next) {  // Only one node
        delete head;
        head = nullptr;
        return;
    }
    
    Node* curr = head;
    while (curr->next->next) {  // Find second-to-last
        curr = curr->next;
    }
    
    delete curr->next;
    curr->next = nullptr;
}
```

**Complexity:** O(n) - must traverse

---

### Doubly Linked (with tail pointer):
```cpp
void deleteTail() {
    if (!tail) return;
    
    if (head == tail) {  // Only one node
        delete head;
        head = tail = nullptr;
        return;
    }
    
    Node* temp = tail;
    tail = tail->prev;
    tail->next = nullptr;
    delete temp;
}
```

**Complexity:** O(1) - have tail pointer!

---

## 5️⃣ SEARCH / FIND

### Both Singly & Doubly (Same):
```cpp
Node* search(int val) {
    Node* curr = head;
    
    while (curr) {
        if (curr->data == val) {
            return curr;
        }
        curr = curr->next;
    }
    
    return nullptr;
}
```

**Complexity:** O(n)
**Pattern:** Traverse and check

---

## 6️⃣ INSERT AT POSITION

### Singly Linked:
```cpp
void insertAt(int pos, int val) {
    if (pos == 0) {
        insertAtHead(val);
        return;
    }
    
    Node* curr = head;
    for (int i = 0; i < pos - 1 && curr; i++) {
        curr = curr->next;
    }
    
    if (!curr) return;  // Position out of bounds
    
    Node* newNode = new Node(val);
    newNode->next = curr->next;
    curr->next = newNode;
}
```

**Pattern:** Find position-1 → Insert after

---

## 7️⃣ DELETE AT POSITION

### Singly Linked:
```cpp
void deleteAt(int pos) {
    if (pos == 0) {
        deleteHead();
        return;
    }
    
    Node* curr = head;
    for (int i = 0; i < pos - 1 && curr; i++) {
        curr = curr->next;
    }
    
    if (!curr || !curr->next) return;
    
    Node* temp = curr->next;
    curr->next = temp->next;
    delete temp;
}
```

**Pattern:** Find position-1 → Delete next

---

## 8️⃣ REVERSE LIST ⭐⭐⭐

### Singly Linked:
```cpp
void reverse() {
    Node* prev = nullptr;
    Node* curr = head;
    Node* next = nullptr;
    
    while (curr) {
        next = curr->next;     // Save next
        curr->next = prev;     // Reverse link
        prev = curr;           // Move prev
        curr = next;           // Move curr
    }
    
    head = prev;
}
```

**Pattern:** Three pointers dance
```
Step 1: [prev=null] [curr] → [next] → ...
Step 2: [prev=null] ← [curr]  [next] → ...
Step 3: [curr] ← [prev]  [curr=next] → ...
```

**MEMORIZE THIS!** Very common on exams!

---

### Doubly Linked:
```cpp
void reverse() {
    Node* curr = head;
    Node* temp = nullptr;
    
    while (curr) {
        temp = curr->prev;
        curr->prev = curr->next;
        curr->next = temp;
        curr = curr->prev;  // Move to "next" (which was prev)
    }
    
    if (temp) {
        head = temp->prev;
    }
}
```

**Pattern:** Swap prev and next for each node

---

## 9️⃣ FIND MIDDLE ⭐⭐⭐

### Singly Linked (Fast & Slow Pointers):
```cpp
Node* findMiddle() {
    Node* slow = head;
    Node* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    return slow;
}
```

**Pattern:** Tortoise & Hare
- Slow moves 1 step
- Fast moves 2 steps
- When fast reaches end, slow is at middle

**MEMORIZE THIS!** Very common technique!

---

## 🔟 DETECT CYCLE ⭐⭐⭐

### Singly Linked (Floyd's Algorithm):
```cpp
bool hasCycle() {
    Node* slow = head;
    Node* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {
            return true;  // Cycle detected
        }
    }
    
    return false;
}
```

**Pattern:** If there's a cycle, fast will catch slow

---

## 1️⃣1️⃣ REMOVE DUPLICATES

### Sorted List:
```cpp
void removeDuplicates() {
    Node* curr = head;
    
    while (curr && curr->next) {
        if (curr->data == curr->next->data) {
            Node* temp = curr->next;
            curr->next = temp->next;
            delete temp;
        } else {
            curr = curr->next;
        }
    }
}
```

**Pattern:** Compare adjacent, delete if same

---

### Unsorted List:
```cpp
void removeDuplicates() {
    Node* curr = head;
    
    while (curr) {
        Node* runner = curr;
        while (runner->next) {
            if (runner->next->data == curr->data) {
                Node* temp = runner->next;
                runner->next = temp->next;
                delete temp;
            } else {
                runner = runner->next;
            }
        }
        curr = curr->next;
    }
}
```

**Pattern:** Nested loop, O(n²)

---

# PART 3: WHEN TO USE WHICH?

## 🤔 Decision Tree

```
START: Given a linked list problem

Do you need to remove/insert in middle frequently?
├─ YES → Do you know the node already?
│   ├─ YES → Use DOUBLY (O(1) removal)
│   └─ NO  → Use SINGLY (still O(n) to find)
└─ NO → Continue

Do you need to access tail frequently?
├─ YES → Use DOUBLY with tail pointer
└─ NO  → Use SINGLY (simpler)

Memory very constrained?
├─ YES → Use SINGLY (less memory)
└─ NO  → Use DOUBLY (easier operations)

Problem explicitly says "singly"?
├─ YES → Use SINGLY (no choice!)
└─ NO  → Your choice based on above
```

---

## 📊 Singly vs Doubly Comparison

| Feature | Singly LL | Doubly LL |
|---------|-----------|-----------|
| **Memory** | Less (1 pointer) | More (2 pointers) |
| **Insert at head** | O(1) | O(1) |
| **Insert at tail** | O(n) | O(1) with tail |
| **Delete at head** | O(1) | O(1) |
| **Delete at tail** | O(n) | O(1) with tail |
| **Delete middle** | O(n) | O(1) if have node |
| **Reverse** | Easy | Medium |
| **Traverse backward** | Can't | Can |
| **Complexity** | Simple | More complex |

---

## ✅ Use SINGLY when:
- ✅ Problem says "singly linked list"
- ✅ Only need forward traversal
- ✅ Memory is very limited
- ✅ Operations are simple (search, insert at head)
- ✅ Learning/practicing basics

## ✅ Use DOUBLY when:
- ✅ Need to remove nodes efficiently
- ✅ Need to traverse backward
- ✅ Implementing LRU cache
- ✅ Need tail operations frequently
- ✅ No memory constraints

---

# PART 4: UNIVERSAL TEMPLATE (Always Write This!)

## 📝 The BASE STRUCTURE (Write this FIRST)

### For Singly Linked List:

```cpp
// 1. STRUCT DEFINITION (Always first!)
struct Node {
    int data;      // Can be any type
    Node* next;
    
    Node(int val) : data(val), next(nullptr) {}
};

// 2. CLASS WITH BASICS
class LinkedList {
private:
    Node* head;
    
public:
    LinkedList() : head(nullptr) {}
    
    ~LinkedList() {
        Node* curr = head;
        while (curr) {
            Node* next = curr->next;
            delete curr;
            curr = next;
        }
    }
    
    // Your operations here...
};
```

### For Doubly Linked List:

```cpp
// 1. STRUCT DEFINITION
struct Node {
    int data;
    Node* prev;
    Node* next;
    
    Node(int val) : data(val), prev(nullptr), next(nullptr) {}
};

// 2. CLASS WITH BASICS
class DoublyLinkedList {
private:
    Node* head;
    Node* tail;
    
public:
    DoublyLinkedList() : head(nullptr), tail(nullptr) {}
    
    ~DoublyLinkedList() {
        Node* curr = head;
        while (curr) {
            Node* next = curr->next;
            delete curr;
            curr = next;
        }
    }
    
    // Your operations here...
};
```

---

## 🎯 EDGE CASES TEMPLATE (Always Check!)

```cpp
void anyOperation() {
    // ALWAYS check these edge cases:
    
    // 1. Empty list
    if (!head) {
        // Handle empty case
        return;
    }
    
    // 2. Single node
    if (!head->next) {
        // Handle single node case
        return;
    }
    
    // 3. Two nodes
    if (head->next && !head->next->next) {
        // Handle two nodes case
        return;
    }
    
    // 4. General case
    // Your main logic here
}
```

---

# PART 5: PROBLEM-SOLVING FRAMEWORK

## 🧠 The 7-Step Method (USE THIS ON EVERY PROBLEM!)

### Step 1: UNDERSTAND (2 minutes)
```
Read problem twice
Identify:
- Input type (singly/doubly?)
- What operation?
- Edge cases mentioned?
- Constraints?
```

### Step 2: DRAW (3 minutes)
```
Draw at least 3 cases:
1. Empty list
2. Single node
3. Three nodes (normal case)

Draw the operation step-by-step!
```

### Step 3: IDENTIFY PATTERN (1 minute)
```
Is this:
- Traversal? → while loop
- Deletion? → Save previous
- Insertion? → Connect forward first
- Two pointers? → Slow/fast
- Reversal? → Three pointers
```

### Step 4: WRITE STRUCTURE (2 minutes)
```cpp
// Write the base:
struct Node { ... };

void operation() {
    // Edge cases
    
    // Main logic
    
    // Update pointers
}
```

### Step 5: HANDLE EDGE CASES (3 minutes)
```cpp
if (!head) { ... }          // Empty
if (!head->next) { ... }    // Single
if (node == head) { ... }   // At head
if (node == tail) { ... }   // At tail
```

### Step 6: WRITE MAIN LOGIC (5 minutes)
```cpp
// Follow the pattern identified in Step 3
```

### Step 7: TRACE & VERIFY (4 minutes)
```
Trace your code with drawn examples
Check all edge cases
Verify pointers updated correctly
```

**Total: 20 minutes** (perfect for exam timing!)

---

# PART 6: COMMON PATTERNS & SCHEMES

## 🎨 Pattern 1: TRAVERSAL

```cpp
Node* curr = head;
while (curr) {
    // Do something with curr
    curr = curr->next;
}
```

**When:** Search, print, count

---

## 🎨 Pattern 2: FIND PREVIOUS

```cpp
Node* prev = nullptr;
Node* curr = head;

while (curr && curr->data != target) {
    prev = curr;
    curr = curr->next;
}

// Now have both prev and curr
```

**When:** Delete by value, insert after

---

## 🎨 Pattern 3: TWO POINTERS (Slow/Fast)

```cpp
Node* slow = head;
Node* fast = head;

while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}

// Slow at middle, or cycle detected
```

**When:** Find middle, detect cycle, nth from end

---

## 🎨 Pattern 4: REVERSAL (Three Pointers)

```cpp
Node* prev = nullptr;
Node* curr = head;
Node* next = nullptr;

while (curr) {
    next = curr->next;
    curr->next = prev;
    prev = curr;
    curr = next;
}

head = prev;
```

**When:** Reverse list, check palindrome

---

## 🎨 Pattern 5: RUNNER (Two Lists)

```cpp
Node* p1 = list1;
Node* p2 = list2;

while (p1 && p2) {
    // Compare or merge
    if (p1->data < p2->data) {
        // Process p1
        p1 = p1->next;
    } else {
        // Process p2
        p2 = p2->next;
    }
}
```

**When:** Merge lists, find intersection

---

# PART 7: EXAM STRATEGY

## 📋 Before You Start Writing:

```
□ Read problem completely
□ Draw 3 examples on paper
□ Identify which pattern to use
□ List edge cases
□ Estimate time needed
```

## ✍️ While Writing:

```
□ Start with struct definition
□ Write edge cases first
□ Comment your logic
□ Use meaningful names (curr, prev, not p, q)
□ Update ALL pointers
```

## ✅ Before Submitting:

```
□ Trace with empty list
□ Trace with single node
□ Trace with normal case
□ Check memory management (delete!)
□ Verify all pointers updated
```

---

# PART 8: QUICK REFERENCE SHEET

## 🚀 Most Common Operations (MEMORIZE!)

### Insert at Head (O(1)):
```cpp
newNode->next = head;
head = newNode;
```

### Delete Head (O(1)):
```cpp
Node* temp = head;
head = head->next;
delete temp;
```

### Find Middle (O(n)):
```cpp
slow = slow->next;
fast = fast->next->next;
```

### Reverse (O(n)):
```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

### Detect Cycle (O(n)):
```cpp
if (slow == fast) return true;
```

---

## ⚠️ Common Mistakes (AVOID!)

```
❌ Forgetting to check if (!head)
❌ Losing reference to rest of list
❌ Not updating tail pointer
❌ Memory leaks (not deleting)
❌ Wrong pointer order
❌ Off-by-one errors in loops
❌ Not handling single node case
```

---

## 💡 Pro Tips

### Tip 1: **Draw BEFORE you code**
```
5 minutes drawing saves 30 minutes debugging
```

### Tip 2: **Check NULL before dereferencing**
```cpp
if (node) {
    node->next = ...;  // Safe
}
```

### Tip 3: **Save before changing**
```cpp
Node* temp = curr->next;  // Save first
curr->next = something;   // Then change
```

### Tip 4: **Connect forward, then backward**
```cpp
newNode->next = curr->next;  // Forward
curr->next = newNode;        // Backward
```

### Tip 5: **Test with empty list first**
```
If it works with empty, usually works with all
```

---

# PART 9: EXAM DAY CHECKLIST

## 📝 What to Write First (No Matter What):

```cpp
// 1. Struct
struct Node {
    int data;
    Node* next;  // (and prev if doubly)
    Node(int val) : data(val), next(nullptr) {}
};

// 2. Edge case checks
if (!head) return;
if (!head->next) { /* single node */ }

// 3. Main logic
Node* curr = head;
while (curr) {
    // Your logic
    curr = curr->next;
}
```

## 🎯 Problem Types & Approach:

| Problem Type | First Step | Key Pattern |
|--------------|-----------|-------------|
| "Insert..." | Draw + identify position | Connect forward first |
| "Delete..." | Draw + find previous | Save → Skip → Delete |
| "Reverse..." | Draw states | Three pointers |
| "Find middle" | Draw movement | Two pointers |
| "Detect cycle" | Draw cycle | Two pointers |
| "Merge..." | Draw both lists | Runner pattern |

---

# PART 10: FINAL SUMMARY

## ✅ Always Do:
1. ✅ Draw first (3-4 nodes minimum)
2. ✅ Check edge cases (empty, single, two nodes)
3. ✅ Use meaningful variable names
4. ✅ Comment your logic
5. ✅ Trace through your code
6. ✅ Check memory management
7. ✅ Verify all pointers updated

## ⚡ Fast Recognition:

```
See "insert" → Think: Connect forward first
See "delete" → Think: Save previous
See "reverse" → Think: Three pointers
See "middle" → Think: Slow/fast
See "cycle" → Think: Slow/fast
See "merge" → Think: Runner
```

## 🎯 Time Management:

```
Understand:  2 min  (10%)
Draw:        3 min  (15%)
Structure:   2 min  (10%)
Edge cases:  3 min  (15%)
Main logic:  5 min  (25%)
Trace:       4 min  (20%)
Check:       1 min  (5%)
────────────────────────
Total:      20 min  (100%)
```

---

## 🏆 YOU'RE READY WHEN:

- [ ] Can draw any operation instantly
- [ ] Know all 5 patterns by heart
- [ ] Can write struct + edge cases automatically
- [ ] Recognize pattern in 30 seconds
- [ ] Can write common operations from memory
- [ ] Know when to use singly vs doubly
- [ ] Remember to check all edge cases

---

**Print this guide and keep it while studying!** 📄

**Good luck on your exam!** 🚀💯
