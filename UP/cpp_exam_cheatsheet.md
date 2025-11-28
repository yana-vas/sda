# C++ Arrays & Matrices Exam Cheat Sheet

## Table of Contents
1. [Must-Do Every Time](#must-do-every-time)
2. [1D Array Templates](#1d-array-templates)
3. [2D Matrix Templates](#2d-matrix-templates)
4. [Essential Helper Functions](#essential-helper-functions)
5. [Common Algorithms](#common-algorithms)
6. [Matrix Operations](#matrix-operations)
7. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
8. [Quick Reference Formulas](#quick-reference-formulas)
9. [Code Snippets Library](#code-snippets-library)

---

## Must-Do Every Time

### Before Writing Any Code
- [ ] Read the entire problem twice
- [ ] Identify: Input format, Output format, Constraints
- [ ] Decide what helper functions you need
- [ ] Check if array needs to be sorted/validated first

### File Structure Template
```cpp
#include <iostream>
using namespace std;  // or use std:: prefix everywhere

const unsigned MAX = 100;  // adjust based on problem

// Helper functions first
// Main logic functions
// main() last

int main() {
    // 1. Declare variables
    // 2. Read input
    // 3. Validate input (if required)
    // 4. Process
    // 5. Output
    return 0;
}
```

### Input Validation Checklist
- [ ] Array size within bounds (1 to MAX)
- [ ] Element values within constraints
- [ ] Matrix dimensions valid
- [ ] Special conditions (sorted, positive, etc.)

---

## 1D Array Templates

### Declaration & Reading
```cpp
const int ARR_MAX_SIZE = 100;

void readArr(int arr[], int& arrSize) {
    cout << "Enter size: ";
    cin >> arrSize;
    cout << "Enter elements: ";
    for (int i = 0; i < arrSize; i++) {
        cin >> arr[i];
    }
}

// In main():
int arr[ARR_MAX_SIZE];
int arrSize = 0;
readArr(arr, arrSize);
```

### Printing
```cpp
void printArr(const int arr[], int arrSize) {
    for (int i = 0; i < arrSize; i++) {
        cout << arr[i] << ' ';
    }
    cout << endl;
}
```

### Passing Arrays to Functions
```cpp
// Read-only (use const)
void process(const int arr[], int size);

// Modifiable (no const)
void modify(int arr[], int& size);  // & for size if it changes
```

---

## 2D Matrix Templates

### Declaration & Reading
```cpp
const size_t MAX_COLS = 20;  // MUST be const for 2D arrays

void inputMatrix(int matrix[][MAX_COLS], size_t size) {
    for (size_t i = 0; i < size; i++) {
        for (size_t j = 0; j < size; j++) {
            cin >> matrix[i][j];
        }
    }
}

// With validation
bool readSquareMatrix(int matrix[][MAX], unsigned size) {
    for (unsigned i = 0; i < size; i++) {
        for (unsigned j = 0; j < size; j++) {
            cin >> matrix[i][j];
            if (matrix[i][j] < 1 || matrix[i][j] > 9) {
                cout << "Invalid element!" << endl;
                return false;
            }
        }
    }
    return true;
}

// In main():
int matrix[MAX_COLS][MAX_COLS];
size_t size;
cin >> size;
if (size < 1 || size > MAX_COLS) {
    cout << "Invalid size!";
    return 1;
}
inputMatrix(matrix, size);
```

### Printing
```cpp
void printMatrix(const int matrix[][MAX_COLS], size_t size) {
    for (size_t i = 0; i < size; i++) {
        for (size_t j = 0; j < size; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }
}
```

### Function Signatures for Matrices
```cpp
// ALWAYS specify column size in parameter
void func(int matrix[][MAX_COLS], size_t rows, size_t cols);
void func(const int matrix[][MAX_COLS], size_t size);  // read-only
```

---

## Essential Helper Functions

### Swap
```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

### Number Properties
```cpp
// Is Prime
bool isPrime(int n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}

// Is Perfect Number (sum of divisors = number)
bool isPerfect(int number) {
    if (number < 6) return false;  // 6 is first perfect number
    int sum = 1;
    for (int i = 2; i <= number / 2; i++) {
        if (number % i == 0) {
            sum += i;
        }
    }
    return sum == number;
}

// Digit Sum
int digitSum(int n) {
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

// Count Digits
int countDigits(int n) {
    int count = 0;
    while (n > 0) {
        count++;
        n /= 10;
    }
    return count;
}

// Square Root (integer, returns -1 if not perfect square)
int squareRoot(int n) {
    for (int i = 1; i * i <= n; i++) {
        if (i * i == n) return i;
    }
    return -1;
}
```

### Array Checks
```cpp
// Is Sorted (ascending)
bool isSorted(const int arr[], unsigned size) {
    for (int i = 1; i < size; i++) {
        if (arr[i - 1] > arr[i]) return false;
    }
    return true;
}

// Contains Element
bool contains(const int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) return true;
    }
    return false;
}

// Count Occurrences
int countOccurrences(const int arr[], int size, int target) {
    int count = 0;
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) count++;
    }
    return count;
}
```

---

## Common Algorithms

### Remove Element at Index (Shift Left)
```cpp
void removeAt(int arr[], int& size, unsigned index) {
    if (index >= size) {
        cout << "Error: index out of bounds";
        return;
    }
    for (int i = index + 1; i < size; i++) {
        arr[i - 1] = arr[i];
    }
    size--;
}
```

### Insert Element at Index (Shift Right)
```cpp
void insertAt(int arr[], int& size, unsigned index, int value) {
    if (index > size) {
        cout << "Error: index out of bounds";
        return;
    }
    for (int i = size; i > index; i--) {
        arr[i] = arr[i - 1];
    }
    arr[index] = value;
    size++;
}
```

### Find Min/Max
```cpp
int findMax(const int arr[], int size) {
    int max = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] > max) max = arr[i];
    }
    return max;
}

int findMin(const int arr[], int size) {
    int min = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] < min) min = arr[i];
    }
    return min;
}

// With index
int findMaxIndex(const int arr[], int size) {
    int maxIdx = 0;
    for (int i = 1; i < size; i++) {
        if (arr[i] > arr[maxIdx]) maxIdx = i;
    }
    return maxIdx;
}
```

### Reverse Array
```cpp
void reverse(int arr[], int size) {
    for (int i = 0; i < size / 2; i++) {
        swap(arr[i], arr[size - 1 - i]);
    }
}
```

### Filter Elements (copy matching to new array)
```cpp
void filterPerfect(const int arr[], int arrSize, int result[], int& resultSize) {
    resultSize = 0;
    for (int i = 0; i < arrSize; i++) {
        if (isPerfect(arr[i])) {
            result[resultSize++] = arr[i];
        }
    }
}
```

---

## Matrix Operations

### Row & Column Sums
```cpp
int getRowSum(const int matrix[][MAX], unsigned size, unsigned row) {
    int sum = 0;
    for (unsigned i = 0; i < size; i++) {
        sum += matrix[row][i];
    }
    return sum;
}

int getColSum(const int matrix[][MAX], unsigned size, unsigned col) {
    int sum = 0;
    for (unsigned i = 0; i < size; i++) {
        sum += matrix[i][col];
    }
    return sum;
}
```

### Diagonal Sums
```cpp
long long getMainDiagonalSum(const int matrix[][MAX], size_t size) {
    long long sum = 0;
    for (size_t i = 0; i < size; i++) {
        sum += matrix[i][i];  // main diagonal: row == col
    }
    return sum;
}

long long getSecondDiagonalSum(const int matrix[][MAX], size_t size) {
    long long sum = 0;
    for (size_t i = 0; i < size; i++) {
        sum += matrix[i][size - 1 - i];  // secondary diagonal
    }
    return sum;
}
```

### Transpose (In-Place)
```cpp
void transpose(int matrix[][MAX], unsigned size) {
    for (unsigned i = 0; i < size; i++) {
        for (unsigned j = i + 1; j < size; j++) {  // j = i + 1 !!!
            swap(matrix[i][j], matrix[j][i]);
        }
    }
}
```

### Rotate 90° Clockwise
```cpp
void rotate90CW(int matrix[][MAX], unsigned size) {
    // Step 1: Transpose
    transpose(matrix, size);
    // Step 2: Reverse each row
    for (unsigned i = 0; i < size; i++) {
        for (unsigned j = 0; j < size / 2; j++) {
            swap(matrix[i][j], matrix[i][size - 1 - j]);
        }
    }
}
```

### Rotate 90° Counter-Clockwise
```cpp
void rotate90CCW(int matrix[][MAX], unsigned size) {
    // Step 1: Transpose
    transpose(matrix, size);
    // Step 2: Reverse each column
    for (unsigned j = 0; j < size; j++) {
        for (unsigned i = 0; i < size / 2; i++) {
            swap(matrix[i][j], matrix[size - 1 - i][j]);
        }
    }
}
```

### Rotate N Times
```cpp
void rotateMatrix(int matrix[][MAX], unsigned size, int times) {
    times = ((times % 4) + 4) % 4;  // handle negative, normalize to 0-3
    for (int i = 0; i < times; i++) {
        rotate90CW(matrix, size);
    }
}
```

### Find Max in Row / Min in Column
```cpp
int findMaxInRow(const int matrix[][MAX], size_t size, size_t row) {
    int max = matrix[row][0];
    for (size_t i = 1; i < size; i++) {
        if (matrix[row][i] > max) max = matrix[row][i];
    }
    return max;
}

int findMinInCol(const int matrix[][MAX], size_t size, size_t col) {
    int min = matrix[0][col];
    for (size_t i = 1; i < size; i++) {
        if (matrix[i][col] < min) min = matrix[i][col];
    }
    return min;
}
```

### Saddle Point
```cpp
// Element that is max in its row AND min in its column
void findSaddlePoints(const int matrix[][MAX], size_t size) {
    for (size_t i = 0; i < size; i++) {
        int rowMax = findMaxInRow(matrix, size, i);
        for (size_t j = 0; j < size; j++) {
            int colMin = findMinInCol(matrix, size, j);
            if (matrix[i][j] == rowMax && matrix[i][j] == colMin) {
                cout << "Saddle point at (" << i << "," << j << "): " << matrix[i][j] << endl;
            }
        }
    }
}
```

### Submatrix Sum
```cpp
int getSubmatrixSum(const int matrix[][MAX], unsigned subSize, 
                    unsigned startRow, unsigned startCol) {
    int sum = 0;
    for (unsigned i = startRow; i < startRow + subSize; i++) {
        for (unsigned j = startCol; j < startCol + subSize; j++) {
            sum += matrix[i][j];
        }
    }
    return sum;
}
```

### Non-Overlapping Submatrix Iteration (Sudoku-style)
```cpp
// For n×n matrix with k×k submatrices where n = k²
bool checkAllSubmatrices(const int matrix[][MAX], unsigned size, unsigned subSize) {
    int targetSum = getSubmatrixSum(matrix, subSize, 0, 0);
    for (unsigned i = 0; i <= size - subSize; i += subSize) {
        for (unsigned j = 0; j <= size - subSize; j += subSize) {
            if (getSubmatrixSum(matrix, subSize, i, j) != targetSum) {
                return false;
            }
        }
    }
    return true;
}
```

### Magic Square Check
```cpp
bool isMagicSquare(const int matrix[][MAX], size_t size) {
    long long targetSum = getMainDiagonalSum(matrix, size);
    
    // Check secondary diagonal
    if (getSecondDiagonalSum(matrix, size) != targetSum) return false;
    
    // Check all rows and columns
    for (size_t i = 0; i < size; i++) {
        if (getRowSum(matrix, size, i) != targetSum) return false;
        if (getColSum(matrix, size, i) != targetSum) return false;
    }
    return true;
}
```

### Matrix Multiplication
```cpp
void multiplyMatrices(const int A[][MAX], const int B[][MAX], 
                      int result[][MAX], size_t size) {
    for (size_t i = 0; i < size; i++) {
        for (size_t j = 0; j < size; j++) {
            result[i][j] = 0;  // IMPORTANT: initialize!
            for (size_t k = 0; k < size; k++) {
                result[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}
```

---

## Common Mistakes to Avoid

### 1. Off-by-One Errors
```cpp
// WRONG
for (int i = 0; i <= size; i++)  // goes one past the end!

// CORRECT
for (int i = 0; i < size; i++)
```

### 2. Forgetting to Pass Size by Reference
```cpp
// WRONG - size changes won't persist
void removeAt(int arr[], int size, unsigned index)

// CORRECT
void removeAt(int arr[], int& size, unsigned index)
```

### 3. Missing const for Read-Only Arrays
```cpp
// WRONG - allows accidental modification
int getSum(int arr[], int size)

// CORRECT
int getSum(const int arr[], int size)
```

### 4. Transpose Loop Bounds
```cpp
// WRONG - swaps twice, ends up unchanged!
for (int i = 0; i < size; i++)
    for (int j = 0; j < size; j++)
        swap(matrix[i][j], matrix[j][i]);

// CORRECT - only upper triangle
for (int i = 0; i < size; i++)
    for (int j = i + 1; j < size; j++)  // j starts at i + 1
        swap(matrix[i][j], matrix[j][i]);
```

### 5. Not Initializing Matrix Multiplication Result
```cpp
// WRONG - result contains garbage + sum
result[i][j] += A[i][k] * B[k][j];

// CORRECT - initialize first
result[i][j] = 0;
for (size_t k = 0; k < size; k++) {
    result[i][j] += A[i][k] * B[k][j];
}
```

### 6. Integer Overflow
```cpp
// WRONG - can overflow for large sums
int sum = 0;

// CORRECT - use long long for sums
long long sum = 0;
```

### 7. Negative Modulo
```cpp
// WRONG - negative % positive can be negative in C++
times % 4

// CORRECT - normalize to positive
((times % 4) + 4) % 4
```

### 8. Removing While Iterating
```cpp
// WRONG - skips elements after removal
for (int i = 0; i < size; i++) {
    if (shouldRemove(arr[i])) {
        removeAt(arr, size, i);
    }
}

// CORRECT - decrement i after removal
for (int i = 0; i < size; i++) {
    if (shouldRemove(arr[i])) {
        removeAt(arr, size, i);
        i--;  // recheck same index
    }
}
```

### 9. Using Wrong Index Variable
```cpp
// WRONG - common typo in nested loops
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; i++) {  // should be j++!
        ...
    }
}
```

### 10. Forgetting Return Statement in Bool Functions
```cpp
// WRONG - undefined behavior if loop completes
bool isSorted(const int arr[], int size) {
    for (int i = 1; i < size; i++) {
        if (arr[i-1] > arr[i]) return false;
    }
    // missing return true!
}

// CORRECT
bool isSorted(const int arr[], int size) {
    for (int i = 1; i < size; i++) {
        if (arr[i-1] > arr[i]) return false;
    }
    return true;  // array is sorted
}
```

---

## Quick Reference Formulas

### Index Calculations
| Position | Formula |
|----------|---------|
| Last element | `arr[size - 1]` |
| Middle element | `arr[size / 2]` |
| Main diagonal | `matrix[i][i]` |
| Secondary diagonal | `matrix[i][size - 1 - i]` |
| Mirror horizontally | `matrix[i][size - 1 - j]` |
| Mirror vertically | `matrix[size - 1 - i][j]` |

### Diagonal Checks
```cpp
bool onMainDiagonal = (row == col);
bool onSecondDiagonal = (row == size - 1 - col);
bool aboveMainDiagonal = (col > row);
bool belowMainDiagonal = (col < row);
```

### Loop Patterns
| Pattern | Inner Loop Start |
|---------|------------------|
| Full matrix | `j = 0` |
| Upper triangle (no diagonal) | `j = i + 1` |
| Upper triangle (with diagonal) | `j = i` |
| Lower triangle (no diagonal) | `j = 0` to `j < i` |
| Lower triangle (with diagonal) | `j = 0` to `j <= i` |

### Rotation Reference
| Rotation | Steps |
|----------|-------|
| 90° CW | transpose → reverse rows |
| 90° CCW | transpose → reverse columns |
| 180° | reverse rows → reverse columns |

---

## Code Snippets Library

### Complete 1D Array Program Template
```cpp
#include <iostream>
using namespace std;

const int MAX_SIZE = 100;

void readArr(int arr[], int& size) {
    cin >> size;
    for (int i = 0; i < size; i++) {
        cin >> arr[i];
    }
}

void printArr(const int arr[], int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << ' ';
    }
    cout << endl;
}

// Add your helper functions here

int main() {
    int arr[MAX_SIZE];
    int size;
    
    readArr(arr, size);
    
    // Validate if needed
    if (size < 1 || size > MAX_SIZE) {
        cout << "Invalid size!" << endl;
        return 1;
    }
    
    // Process and output
    
    return 0;
}
```

### Complete 2D Matrix Program Template
```cpp
#include <iostream>
using namespace std;

const size_t MAX = 20;

void inputMatrix(int matrix[][MAX], size_t size) {
    for (size_t i = 0; i < size; i++) {
        for (size_t j = 0; j < size; j++) {
            cin >> matrix[i][j];
        }
    }
}

void printMatrix(const int matrix[][MAX], size_t size) {
    for (size_t i = 0; i < size; i++) {
        for (size_t j = 0; j < size; j++) {
            cout << matrix[i][j] << ' ';
        }
        cout << endl;
    }
}

// Add your helper functions here

int main() {
    size_t n;
    cin >> n;
    
    if (n < 1 || n > MAX) {
        cout << "Invalid size!" << endl;
        return 1;
    }
    
    int matrix[MAX][MAX];
    inputMatrix(matrix, n);
    
    // Process and output
    
    return 0;
}
```

### Function Overloading Example (from exam)
```cpp
// Same function name, different parameter types
void removeAt(unsigned index, int numbers[], int& size) {
    for (int i = index + 1; i < size; i++) {
        numbers[i - 1] = numbers[i];
    }
    size--;
}

void removeAt(unsigned index, bool flags[], int& size) {
    for (int i = index + 1; i < size; i++) {
        flags[i - 1] = flags[i];
    }
    size--;
}
```

---

## Final Exam Tips

1. **Start with I/O functions** - get input/output working first
2. **Test with simple cases** - try size 2 or 3 before full size
3. **Use meaningful variable names** - `rowSum` not `rs`
4. **Add comments for complex logic** - helps you and the grader
5. **Check edge cases**: empty array, single element, already sorted
6. **Compile often** - don't write 100 lines then compile
7. **Use `const` religiously** - shows you understand the concept
8. **Initialize variables** - especially sums and counters to 0
9. **Return early on errors** - don't let bad input propagate
10. **Double-check loop bounds** - most bugs are off-by-one

---

*Good luck on your exam, Dobri! 🎯*
