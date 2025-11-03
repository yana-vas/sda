# 🌳 LINKED LIST DECISION TREE

## 🎯 START HERE: What Operation Do I Need?

```
                    LINKED LIST PROBLEM
                           ↓
        ┌──────────────────┴──────────────────┐
        ↓                                      ↓
    INSERTION?                            DELETION?
        ↓                                      ↓
   ┌────┴────┐                          ┌─────┴─────┐
   ↓         ↓                          ↓           ↓
  HEAD?    TAIL?                       HEAD?      TAIL?
   ↓         ↓                          ↓           ↓
  O(1)      O(n)                       O(1)   O(n) singly
  EASY      TRAVERSE                   EASY   O(1) doubly


                    LINKED LIST PROBLEM
                           ↓
        ┌──────────────────┴──────────────────┐
        ↓                                      ↓
    SEARCH?                              MANIPULATION?
        ↓                                      ↓
   Traverse                              ┌─────┴─────┐
   O(n)                                  ↓           ↓
   Pattern #1                        REVERSE?     MIDDLE?
                                         ↓           ↓
                                    Pattern #4   Pattern #3
                                    (3 ptrs)     (slow/fast)
```

---

## 📊 DETAILED OPERATION FLOWCHART

```
START: Read problem
   ↓
   ├─ Contains "insert"? ────────────→ INSERTION FLOW
   ├─ Contains "delete/remove"? ─────→ DELETION FLOW
   ├─ Contains "reverse"? ───────────→ REVERSAL FLOW
   ├─ Contains "middle"? ────────────→ TWO-POINTER FLOW
   ├─ Contains "cycle"? ─────────────→ TWO-POINTER FLOW
   ├─ Contains "merge"? ─────────────→ RUNNER FLOW
   └─ Contains "search/find"? ───────→ TRAVERSAL FLOW
```

---

## 🔵 INSERTION FLOW

```
START: Need to insert
   ↓
Where to insert?
   ↓
   ├─ At HEAD? ────→ O(1)
   │                  ↓
   │             newNode->next = head
   │             head = newNode
   │
   ├─ At TAIL? ────→ Singly: O(n)
   │                 Doubly: O(1) with tail ptr
   │                  ↓
   │             Traverse to end
   │             OR use tail pointer
   │
   ├─ At POSITION n? ─→ O(n)
   │                     ↓
   │                Traverse to pos-1
   │                Insert after
   │
   └─ AFTER specific value? ─→ O(n)
                                ↓
                           Find value
                           Insert after
```

---

## 🔴 DELETION FLOW

```
START: Need to delete
   ↓
What to delete?
   ↓
   ├─ HEAD? ──────→ O(1)
   │                 ↓
   │            tmp = head
   │            head = head->next
   │            delete tmp
   │
   ├─ TAIL? ──────→ Singly: O(n)
   │                Doubly: O(1)
   │                 ↓
   │            Find 2nd-to-last
   │            OR use tail->prev
   │
   ├─ By VALUE? ──→ O(n)
   │                 ↓
   │            Find value
   │            Delete (need prev)
   │
   └─ At POSITION n? ─→ O(n)
                         ↓
                    Find position
                    Delete (need prev)
```

---

## 🟢 TRAVERSAL FLOW

```
START: Need to traverse
   ↓
What to do while traversing?
   ↓
   ├─ SEARCH for value? ─→ Compare each node
   ├─ COUNT nodes? ──────→ Increment counter
   ├─ PRINT all? ────────→ Output each node
   ├─ FIND max/min? ─────→ Track best so far
   └─ MODIFY each? ──────→ Update node->data

Template:
Node* curr = head;
while (curr) {
    // Do operation
    curr = curr->next;
}
```

---

## 🟡 REVERSAL FLOW

```
START: Need to reverse
   ↓
Choose method:
   ↓
   ├─ ITERATIVE? ────→ Pattern #4
   │                    ↓
   │               Three pointers:
   │               prev, curr, next
   │               O(n) time, O(1) space
   │
   └─ RECURSIVE? ────→ Less common
                       O(n) time, O(n) space
```

---

## 🟣 TWO-POINTER FLOW

```
START: Two pointers needed
   ↓
What's the goal?
   ↓
   ├─ FIND MIDDLE? ──────→ Slow/Fast
   │                        ↓
   │                   slow moves 1
   │                   fast moves 2
   │                   When fast ends, slow=middle
   │
   ├─ DETECT CYCLE? ─────→ Slow/Fast
   │                        ↓
   │                   If slow==fast → cycle!
   │
   ├─ Nth FROM END? ─────→ Gap pointers
   │                        ↓
   │                   Move fast n steps
   │                   Then move both
   │
   └─ PALINDROME? ───────→ Middle + Reverse
                            ↓
                       Find middle
                       Reverse second half
                       Compare
```

---

## 🟠 RUNNER FLOW (Two Lists)

```
START: Two lists given
   ↓
What to do?
   ↓
   ├─ MERGE sorted? ─────→ Compare and pick smaller
   │                        ↓
   │                   while (p1 && p2)
   │                     pick smaller
   │                     advance that pointer
   │
   ├─ FIND INTERSECTION? ─→ Align lengths
   │                         ↓
   │                    Get lengths
   │                    Move longer ahead
   │                    Find common node
   │
   └─ ZIP/INTERLEAVE? ───→ Alternate picks
                            ↓
                       Take from list1
                       Take from list2
                       Repeat
```

---

## 🎯 SINGLY VS DOUBLY DECISION

```
START: Choose data structure
   ↓
   ├─ Problem says "singly"? ────→ NO CHOICE
   │                                Use Singly
   │
   ├─ Need O(1) removal? ────────→ Doubly (if have node)
   │                                ↓
   │                           Can disconnect in O(1)
   │
   ├─ Need backward traversal? ──→ Doubly (only option)
   │
   ├─ Memory very limited? ───────→ Singly (less memory)
   │
   ├─ Tail operations frequent? ──→ Doubly with tail ptr
   │
   └─ Simple operations only? ────→ Singly (easier)
```

---

## ⏱️ TIME MANAGEMENT FLOW

```
START: 20 minutes for problem
   ↓
Min 1-2: Understand
   ↓
   Read problem
   Identify operation type
   Note constraints
   ↓
Min 3-5: Draw
   ↓
   Draw empty case
   Draw single node case
   Draw 3-4 node case
   Draw step-by-step operation
   ↓
Min 6-7: Structure
   ↓
   Write struct Node { };
   Write edge case checks
   ↓
Min 8-15: Main Logic
   ↓
   Identify pattern (1-5)
   Write main algorithm
   Update all pointers
   ↓
Min 16-19: Trace & Verify
   ↓
   Trace with empty
   Trace with single
   Trace with normal
   Check memory management
   ↓
Min 20: Final check
   ↓
   All pointers updated?
   NULL checks present?
   Memory freed?
   ↓
SUBMIT
```

---

## 🚦 EDGE CASE DECISION

```
Writing any function?
   ↓
   ├─ Check: Is head NULL?
   │    ↓
   │   if (!head) return;
   │
   ├─ Check: Is it single node?
   │    ↓
   │   if (!head->next) { special case }
   │
   ├─ Check: Is it head operation?
   │    ↓
   │   if (node == head) { update head }
   │
   ├─ Check: Is it tail operation?
   │    ↓
   │   if (node == tail) { update tail }
   │
   └─ Check: NULL before dereference?
        ↓
       if (node) { node->next = ... }
```

---

## 💡 PATTERN RECOGNITION FLOWCHART

```
See keywords in problem:
   ↓
   "insert" ────────────────────→ Use insertion pattern
   "delete" "remove" ───────────→ Find prev + delete
   "reverse" ───────────────────→ Three pointer pattern
   "middle" ────────────────────→ Slow/fast pattern
   "cycle" "loop" ──────────────→ Slow/fast pattern
   "merge" "combine" ───────────→ Runner pattern
   "palindrome" ────────────────→ Middle + reverse
   "duplicate" ─────────────────→ Nested or hash
   "sort" ──────────────────────→ Merge sort usually
   "nth from end" ──────────────→ Gap pointers
```

---

## 🎨 DRAWING DECISION

```
Before coding anything:
   ↓
Draw THESE cases (mandatory):
   ↓
   ├─ Empty list: NULL
   ├─ Single node: [A] → NULL
   ├─ Two nodes: [A] → [B] → NULL
   └─ Normal: [A] → [B] → [C] → NULL
   ↓
Draw operation step-by-step:
   ↓
   ├─ Before state
   ├─ Step 1
   ├─ Step 2
   └─ After state
   ↓
NOW you can code!
```

---

## ✅ FINAL SUBMISSION CHECK

```
Before submitting:
   ↓
   □ Struct defined?
   □ Edge cases handled?
   □ Main logic correct?
   □ All pointers updated?
   □ Memory deleted?
   □ NULL checks present?
   □ Traced with examples?
   ↓
   ALL CHECKED? → SUBMIT!
```

---

## 🏆 QUICK RECOGNITION GUIDE

```
Problem mentions:       Use this:
────────────────────   ─────────────────────
"at beginning"      →  Insert/delete head
"at end"            →  Insert/delete tail
"in middle"         →  Find prev, then operate
"reverse"           →  Three pointers
"find middle"       →  Slow/fast pointers
"detect loop"       →  Slow/fast pointers
"merge two"         →  Runner pattern
"palindrome"        →  Middle + reverse
"remove duplicates" →  Compare adjacent/nested
"nth from end"      →  Gap pointers
"intersection"      →  Align + traverse
```

---

**REMEMBER: The flowchart is your friend!** 

**Follow it every time → Success!** ✅
