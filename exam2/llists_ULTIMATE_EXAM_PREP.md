# 📚 ULTIMATE LINKED LIST EXAM PREP - Complete Guide

## 🎯 Table of Contents
1. Past Exam Analysis
2. All Problem Types & Templates
3. Common Patterns
4. Edge Cases Checklist
5. Time Management Strategy
6. Quick Reference Cards

---

# PART 1: PAST EXAM ANALYSIS

## 📊 Problem Type Breakdown (From Last Year)

### Problem Type 1: Get + Replace Pattern
```cpp
// Get element at position X
// Replace all occurrences of that element

Pattern:
1. Find element at position X
2. Traverse list
3. Replace matching values
```

**Key Skills:**
- Access by index
- Traversal
- Conditional replacement
- Handle next pointer carefully

---

### Problem Type 2: Find + Insert Sequence
```cpp
// Find all nodes with value X
// Replace each with sequence of X nodes

Pattern:
1. Traverse with prev pointer
2. When found, create sequence
3. Insert sequence in place
4. Update size
```

**Key Skills:**
- Traverse with previous
- Create node sequences
- Complex insertion
- Memory management
- Size tracking

---

### Problem Type 3: Partition List (NEW)
```cpp
// Separate list by condition
// All nodes < x before nodes >= x

Pattern:
1. Two dummy heads
2. Separate into two lists
3. Connect lists
```

**Key Skills:**
- Two-list technique
- Dummy nodes
- List concatenation
- Preserve order

---

# PART 2: ALL PROBLEM TYPES & TEMPLATES

## 🎨 CATEGORY 1: Access & Retrieval

### Template 1.1: Get Element at Position
```cpp
int getAtPosition(int pos) {
    if (pos < 0 || pos >= size) {
        return -1;  // or throw exception
    }
    
    Node* curr = head;
    int index = 0;
    
    while (curr) {
        if (index == pos) {
            return curr->value;
        }
        index++;
        curr = curr->next;
    }
    
    return -1;  // Not found
}
```

**Complexity:** O(n)
**Edge Cases:**
- Empty list
- pos < 0
- pos >= size
- pos = 0 (head)
- pos = size-1 (tail)

---

### Template 1.2: Get Nth from End
```cpp
int getNthFromEnd(int n) {
    // Two pointer technique
    Node* fast = head;
    Node* slow = head;
    
    // Move fast n steps ahead
    for (int i = 0; i < n && fast; i++) {
        fast = fast->next;
    }
    
    if (!fast) return -1;  // n too large
    
    // Move both until fast reaches end
    while (fast->next) {
        slow = slow->next;
        fast = fast->next;
    }
    
    return slow->value;
}
```

**Edge Cases:**
- n > size
- n = 1 (last element)
- Empty list

---

## 🎨 CATEGORY 2: Find & Replace

### Template 2.1: Replace All Occurrences
```cpp
void replaceAll(int oldVal, int newVal) {
    Node* curr = head;
    
    while (curr) {
        if (curr->value == oldVal) {
            curr->value = newVal;
        }
        curr = curr->next;
    }
}
```

**Edge Cases:**
- Empty list
- Value not found
- All values match
- oldVal == newVal

---

### Template 2.2: Replace with Next Value (EXAM PROBLEM)
```cpp
void replaceWithNext(int X) {
    // Get value at position X
    if (X >= size) return;
    
    Node* curr = head;
    int pos = 0;
    int targetValue;
    
    // Find value at position X
    while (curr && pos < X) {
        pos++;
        curr = curr->next;
    }
    
    if (!curr) return;
    targetValue = curr->value;
    
    // Replace all occurrences with next value
    curr = head;
    while (curr) {
        if (curr->value == targetValue && curr->next) {
            curr->value = curr->next->value;
        }
        curr = curr->next;
    }
}
```

**Edge Cases:**
- X >= size
- Node has no next
- Last node matching
- Empty list

---

### Template 2.3: Replace Node with Sequence (EXAM PROBLEM)
```cpp
void replaceWithSequence(int X) {
    if (!head || X == 1) return;
    
    Node* curr = head;
    Node* prev = nullptr;
    
    while (curr) {
        if (curr->value == X) {
            // Create sequence of X nodes with value 1
            Node* seqHead = new Node(1);
            Node* seqTail = seqHead;
            
            for (int i = 1; i < X; i++) {
                seqTail->next = new Node(1);
                seqTail = seqTail->next;
            }
            
            // Connect sequence
            if (!prev) {
                head = seqHead;
            } else {
                prev->next = seqHead;
            }
            
            seqTail->next = curr->next;
            
            // Update tail if needed
            if (!curr->next) {
                tail = seqTail;
            }
            
            // Delete old node
            Node* temp = curr;
            curr = seqTail;  // Move curr to end of sequence
            delete temp;
            
            size += X - 1;
        }
        
        prev = curr;
        curr = curr->next;
    }
}
```

**Key Points:**
- ✅ Save prev pointer
- ✅ Create sequence
- ✅ Update head if replacing first
- ✅ Update tail if replacing last
- ✅ Update size
- ✅ Free old node

---

## 🎨 CATEGORY 3: Partition & Separation

### Template 3.1: Partition List (NEW - HIGH PRIORITY)
```cpp
Node* partition(Node* head, int x) {
    // Create two dummy heads
    Node lessHead(0);
    Node greaterHead(0);
    
    Node* less = &lessHead;
    Node* greater = &greaterHead;
    
    Node* curr = head;
    while (curr) {
        if (curr->value < x) {
            less->next = curr;
            less = less->next;
        } else {
            greater->next = curr;
            greater = greater->next;
        }
        curr = curr->next;
    }
    
    // CRITICAL: End greater list
    greater->next = nullptr;
    
    // Connect lists
    less->next = greaterHead.next;
    
    return lessHead.next;
}
```

**Edge Cases:**
- Empty list
- All < x
- All >= x
- Single node
- Two nodes

---

### Template 3.2: Separate Even/Odd Values
```cpp
Node* separateEvenOdd(Node* head) {
    Node evenHead(0), oddHead(0);
    Node* even = &evenHead;
    Node* odd = &oddHead;
    
    Node* curr = head;
    while (curr) {
        if (curr->value % 2 == 0) {
            even->next = curr;
            even = even->next;
        } else {
            odd->next = curr;
            odd = odd->next;
        }
        curr = curr->next;
    }
    
    odd->next = nullptr;
    even->next = oddHead.next;
    
    return evenHead.next;
}
```

---

### Template 3.3: Separate Positive/Negative
```cpp
Node* separateBySign(Node* head) {
    Node posHead(0), negHead(0);
    Node* pos = &posHead;
    Node* neg = &negHead;
    
    Node* curr = head;
    while (curr) {
        if (curr->value >= 0) {
            pos->next = curr;
            pos = pos->next;
        } else {
            neg->next = curr;
            neg = neg->next;
        }
        curr = curr->next;
    }
    
    neg->next = nullptr;
    pos->next = negHead.next;
    
    return posHead.next;
}
```

---

## 🎨 CATEGORY 4: Insert Sequences

### Template 4.1: Insert Sequence After Value
```cpp
void insertSequenceAfter(int value, int count) {
    Node* curr = head;
    
    while (curr) {
        if (curr->value == value) {
            // Create sequence
            Node* seqHead = new Node(0);
            Node* seqTail = seqHead;
            
            for (int i = 1; i < count; i++) {
                seqTail->next = new Node(0);
                seqTail = seqTail->next;
            }
            
            // Insert sequence
            seqTail->next = curr->next;
            curr->next = seqHead;
            
            // Update tail if needed
            if (!seqTail->next) {
                tail = seqTail;
            }
            
            size += count;
            curr = seqTail->next;  // Skip sequence
        } else {
            curr = curr->next;
        }
    }
}
```

---

### Template 4.2: Insert N Copies
```cpp
void insertNCopies(int pos, int value, int n) {
    if (pos < 0 || pos > size) return;
    
    // Find insertion point
    Node* curr = head;
    Node* prev = nullptr;
    
    for (int i = 0; i < pos && curr; i++) {
        prev = curr;
        curr = curr->next;
    }
    
    // Create n copies
    for (int i = 0; i < n; i++) {
        Node* newNode = new Node(value);
        
        if (!prev) {
            newNode->next = head;
            head = newNode;
            prev = newNode;
        } else {
            newNode->next = curr;
            prev->next = newNode;
            prev = newNode;
        }
        
        size++;
    }
    
    // Update tail
    if (!curr) {
        tail = prev;
    }
}
```

---

## 🎨 CATEGORY 5: Delete Patterns

### Template 5.1: Delete All Occurrences
```cpp
void deleteAllOccurrences(int value) {
    // Handle head
    while (head && head->value == value) {
        Node* temp = head;
        head = head->next;
        delete temp;
        size--;
    }
    
    if (!head) {
        tail = nullptr;
        return;
    }
    
    // Handle rest
    Node* curr = head;
    while (curr->next) {
        if (curr->next->value == value) {
            Node* temp = curr->next;
            curr->next = temp->next;
            delete temp;
            size--;
        } else {
            curr = curr->next;
        }
    }
    
    // Update tail
    tail = curr;
}
```

**Edge Cases:**
- All nodes match
- Head matches
- Tail matches
- No matches
- Empty list

---

### Template 5.2: Delete Nodes at Positions
```cpp
void deleteAtPositions(vector<int>& positions) {
    sort(positions.begin(), positions.end(), greater<int>());
    
    for (int pos : positions) {
        if (pos < 0 || pos >= size) continue;
        
        if (pos == 0) {
            Node* temp = head;
            head = head->next;
            delete temp;
            size--;
            continue;
        }
        
        Node* curr = head;
        for (int i = 0; i < pos - 1; i++) {
            curr = curr->next;
        }
        
        Node* temp = curr->next;
        curr->next = temp->next;
        delete temp;
        size--;
    }
}
```

---

## 🎨 CATEGORY 6: Transform Operations

### Template 6.1: Reverse
```cpp
void reverse() {
    Node* prev = nullptr;
    Node* curr = head;
    Node* next = nullptr;
    
    tail = head;  // Old head becomes tail
    
    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    
    head = prev;
}
```

---

### Template 6.2: Rotate Left by K
```cpp
void rotateLeft(int k) {
    if (!head || k == 0) return;
    
    k = k % size;
    if (k == 0) return;
    
    // Find k-th node
    Node* curr = head;
    for (int i = 1; i < k; i++) {
        curr = curr->next;
    }
    
    // New head is next of k-th
    Node* newHead = curr->next;
    
    // Old tail points to old head
    tail->next = head;
    
    // k-th node becomes new tail
    tail = curr;
    tail->next = nullptr;
    
    head = newHead;
}
```

---

### Template 6.3: Swap Pairs
```cpp
void swapPairs() {
    Node dummy(0);
    dummy.next = head;
    Node* prev = &dummy;
    
    while (prev->next && prev->next->next) {
        Node* first = prev->next;
        Node* second = first->next;
        
        // Swap
        first->next = second->next;
        second->next = first;
        prev->next = second;
        
        prev = first;
    }
    
    head = dummy.next;
}
```

---

# PART 3: COMMON PATTERNS

## 🔑 Pattern 1: Traverse with Previous
```cpp
Node* curr = head;
Node* prev = nullptr;

while (curr) {
    // Do something with curr and prev
    
    prev = curr;
    curr = curr->next;
}
```

**When to use:**
- Deletion
- Insertion at position
- Replace patterns

---

## 🔑 Pattern 2: Two Pointers (Slow/Fast)
```cpp
Node* slow = head;
Node* fast = head;

while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}

// slow is at middle
```

**When to use:**
- Find middle
- Detect cycle
- Nth from end

---

## 🔑 Pattern 3: Two Dummy Heads
```cpp
Node dummy1(0), dummy2(0);
Node* p1 = &dummy1;
Node* p2 = &dummy2;

while (curr) {
    if (condition) {
        p1->next = curr;
        p1 = p1->next;
    } else {
        p2->next = curr;
        p2 = p2->next;
    }
    curr = curr->next;
}

p2->next = nullptr;
p1->next = dummy2.next;
return dummy1.next;
```

**When to use:**
- Partition
- Separate by condition
- Split into groups

---

## 🔑 Pattern 4: Create Sequence
```cpp
Node* seqHead = new Node(value);
Node* seqTail = seqHead;

for (int i = 1; i < count; i++) {
    seqTail->next = new Node(value);
    seqTail = seqTail->next;
}

// Now insert sequence
```

**When to use:**
- Insert multiple nodes
- Replace with sequence
- Duplicate nodes

---

## 🔑 Pattern 5: Dummy Head (Single)
```cpp
Node dummy(0);
dummy.next = head;
Node* prev = &dummy;

while (prev->next) {
    // Work with prev->next
    // Can delete prev->next safely
}

head = dummy.next;
```

**When to use:**
- Complex deletions
- Avoid head edge case
- Simplify logic

---

# PART 4: EDGE CASES CHECKLIST

## ⚠️ ALWAYS Check These:

```cpp
// 1. Empty list
if (!head) {
    // Handle
    return;
}

// 2. Single node
if (!head->next) {
    // Handle
    return;
}

// 3. Two nodes
if (head->next && !head->next->next) {
    // Handle
    return;
}

// 4. Position out of bounds
if (pos < 0 || pos >= size) {
    return -1;
}

// 5. Value not found
if (!found) {
    return -1;
}

// 6. Null pointer before dereference
if (curr) {
    curr->value = ...;
}

// 7. Update tail when needed
if (!curr->next) {
    tail = curr;
}

// 8. Update head when needed
if (curr == head) {
    head = newHead;
}

// 9. Update size
size += added - removed;

// 10. Free deleted nodes
delete temp;
```

---

# PART 5: COMMON MISTAKES

## ❌ Mistake 1: Losing List Reference
```cpp
// WRONG
head = head->next;  // Lost reference to old head!

// CORRECT
Node* temp = head;
head = head->next;
delete temp;
```

---

## ❌ Mistake 2: Not Ending Second List
```cpp
// WRONG (in partition)
less->next = greaterHead.next;
return lessHead.next;

// CORRECT
greater->next = nullptr;  // ← MUST DO THIS!
less->next = greaterHead.next;
return lessHead.next;
```

---

## ❌ Mistake 3: Forgetting Tail Update
```cpp
// WRONG
void insertAtEnd(int x) {
    Node* newNode = new Node(x);
    tail->next = newNode;
    // Missing: tail = newNode;
}

// CORRECT
void insertAtEnd(int x) {
    Node* newNode = new Node(x);
    tail->next = newNode;
    tail = newNode;  // ← UPDATE TAIL!
}
```

---

## ❌ Mistake 4: Wrong Position Tracking
```cpp
// WRONG
int pos = 1;  // Should start at 0
while (curr) {
    if (pos == X) ...
    pos++;
}

// CORRECT
int pos = 0;  // Start at 0
while (curr) {
    if (pos == X) ...
    pos++;
}
```

---

## ❌ Mistake 5: Not Checking Next Before Access
```cpp
// WRONG
if (curr->value == target) {
    curr->value = curr->next->value;  // Crash if no next!
}

// CORRECT
if (curr->value == target && curr->next) {
    curr->value = curr->next->value;
}
```

---

## ❌ Mistake 6: Memory Leak
```cpp
// WRONG
curr->next = newNode;  // Lost reference to old next!

// CORRECT
newNode->next = curr->next;
curr->next = newNode;
```

---

# PART 6: TIME MANAGEMENT

## ⏱️ 25-Minute Problem Strategy

```
Min 0-2:   Read & Understand (8%)
           - Read twice
           - Identify pattern
           - Note edge cases

Min 3-5:   Draw Examples (12%)
           - Empty list
           - Single node
           - 3-4 nodes
           - Edge case scenario

Min 6-7:   Plan Approach (8%)
           - Which pattern?
           - What pointers needed?
           - Edge cases to handle

Min 8-9:   Write Structure (8%)
           - Edge case checks
           - Variable declarations
           - Loop structure

Min 10-18: Write Code (32%)
           - Main logic
           - Edge case handling
           - Pointer updates

Min 19-23: Test & Trace (20%)
           - Trace with drawn examples
           - Check edge cases
           - Verify pointers

Min 24-25: Final Check (12%)
           - Memory management
           - Size updates
           - Tail/head updates
```

---

# PART 7: PROBLEM RECOGNITION

## 🎯 Quick Recognition Guide

### See "position" or "index" →
```
Template: Access by position
Pattern: Traversal with counter
Time: O(n)
```

### See "all occurrences" →
```
Template: Replace all or Delete all
Pattern: Simple traversal
Time: O(n)
```

### See "replace each X with sequence" →
```
Template: Create and insert sequence
Pattern: Traverse with prev + sequence creation
Time: O(n * m) where m is sequence length
```

### See "partition" or "separate" →
```
Template: Two dummy heads
Pattern: Separate and reconnect
Time: O(n)
```

### See "middle" or "median" →
```
Template: Two pointers
Pattern: Slow/fast
Time: O(n)
```

### See "reverse" →
```
Template: Three pointers
Pattern: prev/curr/next
Time: O(n)
```

### See "remove duplicates" →
```
If sorted: Compare adjacent
If unsorted: Nested loop or hash
Time: O(n) or O(n²)
```

---

# PART 8: EXAM DAY CHECKLIST

## 📋 Before Writing ANY Code:

```
□ Read problem completely
□ Identify problem type
□ Draw 3 examples
□ List edge cases
□ Choose pattern/template
□ Plan pointer strategy
```

## ✍️ While Writing Code:

```
□ Start with edge cases
□ Declare all needed pointers
□ Write loop structure first
□ Add main logic
□ Update all pointers
□ Check memory management
□ Update size if needed
□ Update head/tail if needed
```

## ✅ Before Submitting:

```
□ Traced with empty list
□ Traced with single node
□ Traced with normal case
□ Traced with edge case
□ All pointers updated
□ Memory freed
□ Size correct
□ Tail correct
□ Head correct
```

---

# PART 9: QUICK REFERENCE CARDS

## 🎴 Card 1: Basic Operations

```cpp
// Insert at head - O(1)
newNode->next = head;
head = newNode;

// Insert at tail - O(1) with tail pointer
tail->next = newNode;
tail = newNode;

// Delete head - O(1)
temp = head;
head = head->next;
delete temp;

// Delete tail - O(n) singly, O(1) doubly
// Singly: Find second-to-last
// Doubly: tail = tail->prev; tail->next = nullptr;
```

---

## 🎴 Card 2: Traversal Patterns

```cpp
// Simple traversal
Node* curr = head;
while (curr) {
    // Process curr
    curr = curr->next;
}

// With previous
Node* prev = nullptr;
Node* curr = head;
while (curr) {
    // Process with prev and curr
    prev = curr;
    curr = curr->next;
}

// Two pointers
Node* slow = head;
Node* fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## 🎴 Card 3: Partition Template

```cpp
Node list1(0), list2(0);
Node *p1 = &list1, *p2 = &list2;

while (curr) {
    if (condition) {
        p1->next = curr;
        p1 = p1->next;
    } else {
        p2->next = curr;
        p2 = p2->next;
    }
    curr = curr->next;
}

p2->next = nullptr;        // END LIST 2!
p1->next = list2.next;     // CONNECT
return list1.next;          // RETURN
```

---

## 🎴 Card 4: Sequence Creation

```cpp
// Create sequence of N nodes
Node* seqHead = new Node(value);
Node* seqTail = seqHead;

for (int i = 1; i < N; i++) {
    seqTail->next = new Node(value);
    seqTail = seqTail->next;
}

// Insert sequence after curr
seqTail->next = curr->next;
curr->next = seqHead;
```

---

## 🎴 Card 5: Delete Patterns

```cpp
// Delete node after curr
temp = curr->next;
curr->next = temp->next;
delete temp;

// Delete all matching
while (head && head->value == target) {
    temp = head;
    head = head->next;
    delete temp;
}

curr = head;
while (curr && curr->next) {
    if (curr->next->value == target) {
        temp = curr->next;
        curr->next = temp->next;
        delete temp;
    } else {
        curr = curr->next;
    }
}
```

---

# PART 10: FINAL TIPS

## 💡 The Golden Rules

1. **DRAW FIRST** - 3 minutes drawing saves 10 minutes debugging
2. **Check NULL** - Before every pointer dereference
3. **Save BEFORE changing** - Don't lose references
4. **Update EVERYTHING** - head, tail, size, prev
5. **Test EDGE cases** - Empty, single, boundary

## 🎯 Problem-Solving Mantra

```
1. What pattern is this?
2. What pointers do I need?
3. What edge cases exist?
4. Draw it first
5. Code it carefully
6. Test thoroughly
```

## ⚡ Speed Tips

- Recognize patterns instantly (practice!)
- Use templates (memorize common ones)
- Draw small (3-4 nodes enough)
- Test as you code (not after)
- Skip hard parts, come back

## 🏆 Success Formula

```
Pattern Recognition  (25%)
+ Drawing Skills     (25%)
+ Template Knowledge (25%)
+ Edge Case Handling (25%)
= Exam Success      (100%)
```

---

**GOOD LUCK ON YOUR EXAM!** 🚀

**Remember:** 
- These problems follow patterns
- Draw before coding
- Check your edge cases
- You've got this! 💪

---

## 📚 All Your Resources

Save these files:
1. This complete guide
2. EXAM_GUIDE.md - All operations
3. CHEAT_SHEET.md - One-page reference
4. partition_list_guide.md - Partition detail
5. OPTIMIZATION_TRICKS.md - Advanced thinking

**Print the cheat sheets and keep them while studying!** 📄✨
