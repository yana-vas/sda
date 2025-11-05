# 📄 LINKED LIST - ONE PAGE CHEAT SHEET

## 🏗️ ALWAYS WRITE FIRST:

```cpp
struct Node {
    int data;
    Node* next;  // +prev for doubly
    Node(int val) : data(val), next(nullptr) {}
};
```

## ⚠️ ALWAYS CHECK:
```cpp
if (!head) return;           // Empty
if (!head->next) { ... }     // Single node
```

---

## 🎨 THE 5 PATTERNS (MEMORIZE!)

### 1. TRAVERSAL
```cpp
Node* curr = head;
while (curr) {
    // Process curr
    curr = curr->next;
}
```

### 2. FIND PREVIOUS
```cpp
Node* prev = nullptr;
Node* curr = head;
while (curr && curr->data != target) {
    prev = curr;
    curr = curr->next;
}
```

### 3. TWO POINTERS (Slow/Fast)
```cpp
Node* slow = head;
Node* fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
// slow at middle OR cycle detected
```

### 4. REVERSE
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

### 5. RUNNER (Two Lists)
```cpp
while (p1 && p2) {
    if (p1->data < p2->data) {
        p1 = p1->next;
    } else {
        p2 = p2->next;
    }
}
```

---

## ⚡ COMMON OPERATIONS

| Operation | Code | Time |
|-----------|------|------|
| **Insert Head** | `new->next=head; head=new;` | O(1) |
| **Delete Head** | `tmp=head; head=head->next; delete tmp;` | O(1) |
| **Insert Tail** | Traverse to end, then insert | O(n) |
| **Delete Tail** | Find 2nd-to-last, delete last | O(n) |
| **Search** | Traverse until found | O(n) |

---

## 🔄 WHEN TO USE WHICH?

**Singly:** Simple, less memory, only forward
**Doubly:** Remove middle O(1), traverse backward, tail operations O(1)

---

## 🎯 PROBLEM → PATTERN

```
"Insert"     → Connect forward first
"Delete"     → Find previous, save before delete
"Reverse"    → Pattern 4 (three pointers)
"Middle"     → Pattern 3 (slow/fast)
"Cycle"      → Pattern 3 (slow==fast?)
"Merge"      → Pattern 5 (runner)
"Palindrome" → Middle + Reverse
```

---

## 💡 GOLDEN RULES

1. **DRAW FIRST** (3 nodes minimum)
2. **Connect FORWARD, then BACKWARD**
3. **SAVE before changing pointers**
4. **Check NULL before dereferencing**
5. **Test with empty list first**

---

## ⚠️ AVOID THESE MISTAKES

```
❌ head = newNode; newNode->next = head;  // Lost list!
✅ newNode->next = head; head = newNode;

❌ curr->next = ...;  // If curr is NULL → CRASH
✅ if (curr) curr->next = ...;

❌ Node* temp = curr; curr = curr->next; delete curr;
✅ Node* temp = curr; curr = curr->next; delete temp;
```

---

## 📝 EXAM STRATEGY (20 min total)

1. **Understand** (2 min) - Read twice
2. **Draw** (3 min) - Empty, single, 3 nodes
3. **Pattern** (1 min) - Which of the 5?
4. **Structure** (2 min) - Struct + edge cases
5. **Logic** (8 min) - Write main code
6. **Trace** (4 min) - Walk through examples

---

## 🔑 QUICK DECISIONS

```
Need to remove middle often?
  → Doubly LL (O(1) if have node)

Only insert/delete at ends?
  → Singly LL (simpler)

Need to traverse backward?
  → Doubly LL (only option)

Problem says "singly"?
  → No choice, use singly!
```

---

## ✅ BEFORE SUBMITTING

□ Traced with empty list?
□ Traced with single node?
□ All pointers updated?
□ Memory deleted?
□ NULL checks?

---

**Remember:** If you can DRAW it, you can CODE it! 🎨→💻
