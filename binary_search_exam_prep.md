# Binary Search Exam Preparation Guide

## Table of Contents
1. [Core Templates](#core-templates)
2. [Problem Patterns](#problem-patterns)
3. [Visual Examples](#visual-examples)
4. [Common Mistakes](#common-mistakes)
5. [Problem Solutions](#problem-solutions)
6. [Quick Reference Cheat Sheet](#cheat-sheet)

---

## Core Templates

### 1. Classic Binary Search (Find Exact Value)
```cpp
int binarySearch(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Prevents overflow
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;  // Not found
}
```

### 2. Lower Bound (First >= target)
```cpp
int lowerBound(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

### 3. Upper Bound (First > target)
```cpp
int upperBound(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

### 4. Binary Search on Answer (Koko-style problems)
```cpp
bool canAchieve(long long answer, /* other params */) {
    // Check if 'answer' is feasible
    // Return true if we can achieve the goal with this answer
}

long long binarySearchOnAnswer(long long left, long long right) {
    long long result = right;
    
    while (left <= right) {
        long long mid = left + (right - left) / 2;
        
        if (canAchieve(mid)) {
            result = mid;           // Save this answer
            right = mid - 1;        // Try to find smaller
        } else {
            left = mid + 1;         // Need larger answer
        }
    }
    return result;
}
```

---

## Problem Patterns

### Pattern 1: Find Target in Sorted Array
**When to use:** Array is sorted, finding exact value
**Template:** Classic binary search
**Example:** Search Insert Position, Find First and Last Position

### Pattern 2: Find Boundary (First/Last Occurrence)
**When to use:** Finding first or last position satisfying a condition
**Template:** Lower/Upper bound variants

```cpp
// Find FIRST occurrence
int findFirst(const vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Keep searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return result;
}

// Find LAST occurrence
int findLast(const vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            left = mid + 1;  // Keep searching right
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### Pattern 3: Binary Search on Answer Space (MOST COMMON IN EXAMS!)
**When to use:** "Minimize/maximize something" or "find minimum/maximum value that satisfies condition"
**Key idea:** Instead of searching in array, search for the answer!

**Classic structure:**
1. Define search space [left, right]
2. Write a `canAchieve(mid)` function that checks if `mid` is valid
3. Use binary search to find optimal answer

**Examples:** Koko Eating Bananas, Split Array Largest Sum, Capacity To Ship Packages

```cpp
// Template for "Find MINIMUM value that works"
long long minimizeAnswer(long long left, long long right) {
    long long result = right;
    
    while (left <= right) {
        long long mid = left + (right - left) / 2;
        
        if (canAchieve(mid)) {
            result = mid;
            right = mid - 1;  // Try smaller
        } else {
            left = mid + 1;   // Need bigger
        }
    }
    return result;
}

// Template for "Find MAXIMUM value that works"
long long maximizeAnswer(long long left, long long right) {
    long long result = left;
    
    while (left <= right) {
        long long mid = left + (right - left) / 2;
        
        if (canAchieve(mid)) {
            result = mid;
            left = mid + 1;   // Try larger
        } else {
            right = mid - 1;  // Need smaller
        }
    }
    return result;
}
```

### Pattern 4: Search in Rotated/Modified Array
**When to use:** Array has special structure (rotated, mountain, etc.)
**Key idea:** Identify which part is sorted, then decide where to search

```cpp
int searchRotated(const vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) return mid;
        
        // Determine which half is sorted
        if (arr[left] <= arr[mid]) {  // Left half is sorted
            if (arr[left] <= target && target < arr[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {  // Right half is sorted
            if (arr[mid] < target && target <= arr[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    return -1;
}
```

### Pattern 5: Find Peak Element
**When to use:** Finding local maximum
**Key idea:** Compare mid with neighbors

```cpp
int findPeakElement(const vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] < arr[mid + 1]) {
            left = mid + 1;  // Peak is on the right
        } else {
            right = mid;     // Peak is on the left or at mid
        }
    }
    return left;
}
```

---

## Visual Examples

### Example 1: Classic Binary Search
```
Array: [1, 3, 5, 7, 9, 11, 13]  Target: 7

Step 1: left=0, right=6, mid=3
        [1, 3, 5, |7|, 9, 11, 13]
        arr[3]=7 == target ✓ Found!

Example 2: Target not found (4)
Step 1: [1, 3, |5|, 7, 9, 11, 13]  left=0, right=6, mid=3
        arr[3]=5 < 4? No, 5>4, so right=2

Step 2: [1, |3|, 5]  left=0, right=2, mid=1
        arr[1]=3 < 4? Yes, so left=2

Step 3: [5]  left=2, right=2, mid=2
        arr[2]=5 < 4? No, 5>4, so right=1

Step 4: left=2, right=1, left>right → Stop, return -1
```

### Example 2: Binary Search on Answer (Koko Problem)
```
Problem: Koko has N piles of bananas [3, 6, 7, 11]
She has H=8 hours to eat all. Find minimum eating speed K.

Visualization of speed K:
K=1:  [3hrs, 6hrs, 7hrs, 11hrs] = 27hrs total ✗ (too slow)
K=2:  [2hrs, 3hrs, 4hrs, 6hrs]  = 15hrs total ✗
K=3:  [1hr,  2hrs, 3hrs, 4hrs]  = 10hrs total ✗
K=4:  [1hr,  2hrs, 2hrs, 3hrs]  = 8hrs total ✓ (works!)
K=5:  [1hr,  2hrs, 2hrs, 3hrs]  = 8hrs total ✓

Binary Search Process:
Search space: [1, 11]  (min=1, max=max(piles))

Step 1: mid=6
        Can eat with K=6? [1,1,2,2]=6hrs < 8 ✓
        Try smaller: right=5

Step 2: mid=3
        Can eat with K=3? [1,2,3,4]=10hrs > 8 ✗
        Need faster: left=4

Step 3: mid=4
        Can eat with K=4? [1,2,2,3]=8hrs == 8 ✓
        Try smaller: right=3

Step 4: left=4, right=3 → Stop
        Answer: 4
```

### Example 3: First and Last Position
```
Array: [5, 7, 7, 8, 8, 8, 10]  Target: 8

Finding FIRST 8:
[5, 7, 7, |8|, 8, 8, 10]  mid=3, found 8
                          save result=3, search left
[5, 7, |7|]              mid=1, 7<8, search right
    [|7|]                mid=2, 7<8, search right
Stop → First position: 3

Finding LAST 8:
[5, 7, 7, |8|, 8, 8, 10]  mid=3, found 8
                          save result=3, search right
        [8, |8|, 10]     mid=5, found 8
                          save result=5, search right
            [|10|]       mid=6, 10>8, search left
Stop → Last position: 5

Answer: [3, 5]
```

---

## Common Mistakes

### ❌ Mistake 1: Integer Overflow
```cpp
// WRONG
int mid = (left + right) / 2;  // Can overflow if left+right > INT_MAX

// CORRECT
int mid = left + (right - left) / 2;
```

### ❌ Mistake 2: Infinite Loop with `left < right`
```cpp
// Be careful with condition and updates
while (left < right) {
    int mid = left + (right - left) / 2;
    if (condition) {
        left = mid;      // ❌ If left==mid, infinite loop!
    } else {
        right = mid - 1;
    }
}

// FIX: Use left = mid + 1
while (left < right) {
    int mid = left + (right - left) / 2;
    if (condition) {
        left = mid + 1;  // ✓ Always makes progress
    } else {
        right = mid;
    }
}
```

### ❌ Mistake 3: Wrong Boundary Updates
```cpp
// When finding MINIMUM valid answer:
if (canAchieve(mid)) {
    result = mid;
    right = mid - 1;  // ✓ Try smaller
} else {
    left = mid + 1;   // ✓ Need larger
}

// NOT:
if (canAchieve(mid)) {
    left = mid + 1;   // ❌ Wrong! We want smaller values
}
```

### ❌ Mistake 4: Off-by-One Errors
```cpp
// Array size: n
int left = 0;
int right = n - 1;  // ✓ For inclusive right
// OR
int right = n;      // ✓ For exclusive right (used in lower_bound)

// Then use appropriate loop condition:
while (left <= right) { }  // For inclusive
while (left < right) { }   // For exclusive
```

### ❌ Mistake 5: Forgetting Edge Cases
```cpp
// Always check:
// - Empty array
// - Single element
// - Target not in array
// - All elements same
// - Target at boundaries

if (arr.empty()) return -1;
if (target < arr[0] || target > arr.back()) return -1;
```

---

## Problem Solutions

### 1. Koko Eating Bananas (EXAM FAVORITE!)
```cpp
class Solution {
private:
    // Check if Koko can eat all bananas in H hours at speed K
    bool canFinish(vector<int>& piles, int H, long long K) {
        long long hours = 0;
        for (int pile : piles) {
            hours += (pile + K - 1) / K;  // Ceiling division
            if (hours > H) return false;   // Early exit
        }
        return hours <= H;
    }
    
public:
    int minEatingSpeed(vector<int>& piles, int H) {
        // Search space: [1, max pile size]
        long long left = 1;
        long long right = *max_element(piles.begin(), piles.end());
        long long result = right;
        
        while (left <= right) {
            long long mid = left + (right - left) / 2;
            
            if (canFinish(piles, H, mid)) {
                result = mid;
                right = mid - 1;  // Try slower speed
            } else {
                left = mid + 1;   // Need faster speed
            }
        }
        return result;
    }
};
```

### 2. Search Insert Position
```cpp
int searchInsert(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    // If not found, left is the insertion position
    return left;
}
```

### 3. First Bad Version
```cpp
bool isBadVersion(int version);  // API provided

int firstBadVersion(int n) {
    int left = 1, right = n;
    int result = n;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (isBadVersion(mid)) {
            result = mid;
            right = mid - 1;  // Search for earlier bad version
        } else {
            left = mid + 1;   // All versions <= mid are good
        }
    }
    return result;
}
```

### 4. Sqrt(x)
```cpp
int mySqrt(int x) {
    if (x == 0) return 0;
    
    long long left = 1, right = x;
    long long result = 1;
    
    while (left <= right) {
        long long mid = left + (right - left) / 2;
        long long square = mid * mid;
        
        if (square == x) {
            return mid;
        } else if (square < x) {
            result = mid;
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### 5. Find First and Last Position
```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> result = {-1, -1};
    if (nums.empty()) return result;
    
    // Find first position
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            result[0] = mid;
            right = mid - 1;  // Continue searching left
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // Find last position
    left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            result[1] = mid;
            left = mid + 1;  // Continue searching right
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

### 6. Search in Rotated Sorted Array
```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) return mid;
        
        // Determine which half is sorted
        if (nums[left] <= nums[mid]) {  // Left half is sorted
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;  // Target in sorted left half
            } else {
                left = mid + 1;   // Target in right half
            }
        } else {  // Right half is sorted
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;   // Target in sorted right half
            } else {
                right = mid - 1;  // Target in left half
            }
        }
    }
    return -1;
}
```

### 7. Find Peak Element
```cpp
int findPeakElement(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] > nums[mid + 1]) {
            right = mid;  // Peak is on the left (or at mid)
        } else {
            left = mid + 1;  // Peak is on the right
        }
    }
    return left;
}
```

### 8. Search 2D Matrix
```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if (matrix.empty() || matrix[0].empty()) return false;
    
    int m = matrix.size(), n = matrix[0].size();
    int left = 0, right = m * n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int midValue = matrix[mid / n][mid % n];  // Convert 1D to 2D
        
        if (midValue == target) {
            return true;
        } else if (midValue < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}
```

### 9. Split Array Largest Sum (Hard)
```cpp
class Solution {
private:
    bool canSplit(vector<int>& nums, int m, long long maxSum) {
        int subarrays = 1;
        long long currentSum = 0;
        
        for (int num : nums) {
            if (currentSum + num > maxSum) {
                subarrays++;
                currentSum = num;
                if (subarrays > m) return false;
            } else {
                currentSum += num;
            }
        }
        return true;
    }
    
public:
    int splitArray(vector<int>& nums, int m) {
        long long left = *max_element(nums.begin(), nums.end());
        long long right = accumulate(nums.begin(), nums.end(), 0LL);
        long long result = right;
        
        while (left <= right) {
            long long mid = left + (right - left) / 2;
            
            if (canSplit(nums, m, mid)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return result;
    }
};
```

### 10. Median of Two Sorted Arrays (Hard)
```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    if (nums1.size() > nums2.size()) {
        return findMedianSortedArrays(nums2, nums1);
    }
    
    int m = nums1.size(), n = nums2.size();
    int left = 0, right = m;
    
    while (left <= right) {
        int partition1 = (left + right) / 2;
        int partition2 = (m + n + 1) / 2 - partition1;
        
        int maxLeft1 = (partition1 == 0) ? INT_MIN : nums1[partition1 - 1];
        int minRight1 = (partition1 == m) ? INT_MAX : nums1[partition1];
        int maxLeft2 = (partition2 == 0) ? INT_MIN : nums2[partition2 - 1];
        int minRight2 = (partition2 == n) ? INT_MAX : nums2[partition2];
        
        if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
            if ((m + n) % 2 == 0) {
                return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0;
            } else {
                return max(maxLeft1, maxLeft2);
            }
        } else if (maxLeft1 > minRight2) {
            right = partition1 - 1;
        } else {
            left = partition1 + 1;
        }
    }
    return 0.0;
}
```

---

## Quick Reference Cheat Sheet

### When to Use Each Template

| Problem Type | Template | Loop Condition | Key Points |
|-------------|----------|----------------|------------|
| Find exact value | Classic BS | `left <= right` | Return when found |
| Find first occurrence | Modified BS | `left <= right` | Save result, search left |
| Find last occurrence | Modified BS | `left <= right` | Save result, search right |
| Find minimum valid answer | BS on answer | `left <= right` | Minimize, update right |
| Find maximum valid answer | BS on answer | `left <= right` | Maximize, update left |
| Find insertion position | Classic BS | `left <= right` | Return left at end |
| Rotated array | Modified BS | `left <= right` | Check which half sorted |
| Peak element | Modified BS | `left < right` | Compare with neighbor |

### Time Complexity
- Binary Search: O(log n)
- Binary Search on Answer: O(n log m) where m is search space
- 2D Matrix: O(log(m×n))

### Space Complexity
- Iterative: O(1)
- Recursive: O(log n) stack space

### Quick Decision Tree

```
Is array sorted?
├─ YES → Can you use classic binary search?
│         ├─ YES → Use template #1
│         └─ NO → Finding boundary/occurrence?
│                  └─ Use template #2 or #3
│
└─ NO → Is array rotated/has special structure?
         ├─ YES → Use template #4
         └─ NO → Are you searching for an answer (not in array)?
                  └─ Use template #5 (BS on answer)
```

### Common Test Patterns

1. **Koko-style problems**: Binary search on answer space
   - Key: Define can we achieve X in Y time/resources?
   
2. **Boundary problems**: Find first/last occurrence
   - Key: Save result and keep searching
   
3. **Modified array**: Rotated, peak finding
   - Key: Identify which part has properties you need

4. **2D to 1D**: Treat 2D matrix as 1D array
   - Key: index → (index/cols, index%cols)

### Must Remember Formulas

```cpp
// Ceiling division
(a + b - 1) / b  // or  (a - 1) / b + 1

// Convert 1D index to 2D
row = index / numCols
col = index % numCols

// Convert 2D to 1D
index = row * numCols + col

// Avoid overflow in mid calculation
mid = left + (right - left) / 2
```

### Pre-Exam Checklist

- [ ] Can write classic binary search from memory
- [ ] Know when to use `left <= right` vs `left < right`
- [ ] Understand lower_bound vs upper_bound
- [ ] Can implement binary search on answer
- [ ] Know how to avoid integer overflow
- [ ] Practiced all problem patterns
- [ ] Understand common pitfalls
- [ ] Can debug infinite loops
- [ ] Tested edge cases (empty, single element, not found)

### Last-Minute Tips

1. **Always prevent overflow**: Use `left + (right - left) / 2`
2. **Watch your boundaries**: Inclusive vs exclusive right
3. **Koko problems**: Think "minimize/maximize something"
4. **Save results**: When finding first/last, save before continuing
5. **Test with**: Empty array, single element, target at boundaries
6. **Rotated arrays**: Always check which half is sorted first
7. **Don't forget**: Edge cases make or break your grade!

Good luck on your exam! 🚀
