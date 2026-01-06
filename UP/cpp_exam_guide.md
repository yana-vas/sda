# 🎯 C++ Exam Ultimate Guide - CHEAT SHEET

> **Максимално време:** 1 час | **Език:** C++ | **Цел:** 100 точки

---

## 📋 Table of Contents

1. [Бързи Шаблони (Quick Templates)](#-бързи-шаблони)
2. [Must-Have Функции](#-must-have-функции)
3. [Разпознаване на Задачи](#-разпознаване-на-задачи)
4. [Динамична Памет](#-динамична-памет)
5. [Указатели (Pointers)](#-указатели-pointers)
6. [Символни Низове (C-Strings)](#-символни-низове-c-strings)
7. [Масиви и Матрици](#-масиви-и-матрици)
8. [Побитови Операции](#-побитови-операции)
9. [Сортиране и Търсене](#-сортиране-и-търсене)
10. [Числови Операции](#-числови-операции)
11. [Готови Решения на Типични Задачи](#-готови-решения)
12. [Възможни Изпитни Задачи](#-възможни-изпитни-задачи)

---

## 🚀 Бързи Шаблони

### Основен Template за Изпит
```cpp
#include <iostream>
using namespace std;

const size_t MAX_SIZE = 1024;

int main() {
    // Твоя код тук
    return 0;
}
```

### Template за Функция с Динамична Памет
```cpp
// Връща нов масив - CALLER трябва да delete[]!
int* createArray(int size) {
    int* arr = new int[size];
    // инициализация...
    return arr;
}

// Използване:
int* result = createArray(10);
// ... работа с result ...
delete[] result; // НЕ ЗАБРАВЯЙ!
```

### Template за Работа със Стрингове
```cpp
void processString(const char* input, char* output) {
    if (!input || !output) return;
    
    size_t read = 0, write = 0;
    while (input[read]) {
        // обработка...
        output[write++] = input[read++];
    }
    output[write] = '\0'; // ВИНАГИ терминирай!
}
```

---

## 📚 Must-Have Функции

### 1. String Functions (НАУЧИ НАИЗУСТ!)

```cpp
// ========== strlen ==========
size_t myStrLen(const char* str) {
    if (!str) return 0;
    size_t len = 0;
    while (str[len]) len++;
    return len;
}

// ========== strcpy ==========
void myStrCpy(char* dest, const char* src) {
    if (!dest || !src) return;
    size_t i = 0;
    while (src[i]) {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

// ========== strcat ==========
void myStrCat(char* dest, const char* src) {
    if (!dest || !src) return;
    size_t write = myStrLen(dest);
    size_t read = 0;
    while (src[read]) {
        dest[write++] = src[read++];
    }
    dest[write] = '\0';
}

// ========== strcmp ==========
int myStrCmp(const char* first, const char* second) {
    if (!first || !second) return -128;
    size_t i = 0;
    while (first[i] && first[i] == second[i]) i++;
    return first[i] - second[i];
    // < 0: first < second
    // = 0: equal
    // > 0: first > second
}

// ========== strchr (намиране на символ) ==========
int findChar(const char* str, char c) {
    if (!str) return -1;
    for (size_t i = 0; str[i]; i++) {
        if (str[i] == c) return i;
    }
    return -1;
}

// ========== strstr (намиране на подстринг) ==========
int findSubstring(const char* text, const char* pattern) {
    if (!text || !pattern) return -1;
    size_t textLen = myStrLen(text);
    size_t patLen = myStrLen(pattern);
    
    for (size_t i = 0; i + patLen <= textLen; i++) {
        bool found = true;
        for (size_t j = 0; j < patLen; j++) {
            if (text[i + j] != pattern[j]) {
                found = false;
                break;
            }
        }
        if (found) return i;
    }
    return -1;
}
```

### 2. Character Classification (МНОГО ВАЖНО!)

```cpp
// ========== Проверки за символи ==========
bool isDigit(char c)      { return c >= '0' && c <= '9'; }
bool isLower(char c)      { return c >= 'a' && c <= 'z'; }
bool isUpper(char c)      { return c >= 'A' && c <= 'Z'; }
bool isLetter(char c)     { return isLower(c) || isUpper(c); }
bool isAlphaNum(char c)   { return isLetter(c) || isDigit(c); }
bool isWhitespace(char c) { return c == ' ' || c == '\t' || c == '\n'; }
bool isPunctuation(char c){ return c == '.' || c == ',' || c == '!' || c == '?' || c == ';' || c == ':'; }

// ========== Конверсии ==========
char toLower(char c) { return isUpper(c) ? c + ('a' - 'A') : c; }
char toUpper(char c) { return isLower(c) ? c - ('a' - 'A') : c; }
int charToDigit(char c) { return isDigit(c) ? c - '0' : -1; }
char digitToChar(int d) { return (d >= 0 && d <= 9) ? '0' + d : '\0'; }
```

### 3. Number Functions

```cpp
// ========== Брой цифри ==========
int countDigits(int n) {
    if (n == 0) return 1;
    if (n < 0) n = -n;
    int count = 0;
    while (n > 0) {
        count++;
        n /= 10;
    }
    return count;
}

// ========== Обръщане на число ==========
int reverseNumber(int n) {
    int reversed = 0;
    bool negative = n < 0;
    if (negative) n = -n;
    while (n > 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }
    return negative ? -reversed : reversed;
}

// ========== k-та цифра (от дясно, 0-indexed) ==========
int getDigitAt(int n, int k) {
    if (n < 0) n = -n;
    for (int i = 0; i < k; i++) n /= 10;
    return n % 10;
}

// ========== Сума на цифрите ==========
int digitSum(int n) {
    if (n < 0) n = -n;
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

// ========== Просто число ==========
bool isPrime(int n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}

// ========== GCD (Евклид) ==========
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// ========== LCM ==========
int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}

// ========== Power ==========
long long power(int base, int exp) {
    long long result = 1;
    for (int i = 0; i < exp; i++) {
        result *= base;
    }
    return result;
}
```

---

## 🔍 Разпознаване на Задачи

### Таблица за Бързо Разпознаване

| Ключови думи в условието | Какво да използваш |
|--------------------------|-------------------|
| "динамична памет", "ТОЧНА ГОЛЕМИНА" | `new[]` + `delete[]` |
| "връща нов стринг" | `new char[size + 1]` |
| "замени", "replace" | Итерация + копиране |
| "брой срещания" | Counter variable |
| "подстринг", "инфикс" | Nested loops |
| "сортиран", "подреди" | Bubble/Selection sort |
| "двоично", "бит" | Побитови операции |
| "масив от масиви" | `int**` (2D динамичен) |
| "матрица NxM" | 2D масив |
| "транспониране" | `result[j][i] = matrix[i][j]` |
| "palindrom" | Сравни от двата края |
| "лексикографски" | strcmp логика |
| "prefix/suffix" | Сравни от началото/края |

### Checklist Преди Да Пишеш

1. ✅ **Валидация на входа** - `if (!ptr) return;`
2. ✅ **Терминираща нула** - `str[i] = '\0'`
3. ✅ **delete[]** - Ако има `new[]`
4. ✅ **Размер на резултата** - Изчисли преди `new`
5. ✅ **Edge cases** - Празен стринг, 0, отрицателни

---

## 💾 Динамична Памет

### Stack vs Heap

```cpp
// STACK - автоматично се изтрива
int x = 5;              // на стека
int arr[10];            // на стека (фиксиран размер)

// HEAP - ТИ трябва да изтриеш!
int* p = new int;       // един int на heap-а
int* arr = new int[n];  // масив на heap-а (n може да е променлива!)

delete p;               // за единичен елемент
delete[] arr;           // за масив - ВИНАГИ []!
```

### Динамичен 1D Масив

```cpp
// Създаване
int n;
cin >> n;
int* arr = new int[n];

// Инициализация
for (int i = 0; i < n; i++) {
    arr[i] = 0;
}

// Изтриване
delete[] arr;
arr = nullptr; // добра практика
```

### Динамична 2D Матрица

```cpp
// ========== Създаване NxM матрица ==========
int** createMatrix(int rows, int cols) {
    int** matrix = new int*[rows];
    for (int i = 0; i < rows; i++) {
        matrix[i] = new int[cols];
    }
    return matrix;
}

// ========== Изтриване ==========
void deleteMatrix(int** matrix, int rows) {
    for (int i = 0; i < rows; i++) {
        delete[] matrix[i];
    }
    delete[] matrix;
}

// ========== Използване ==========
int main() {
    int n, m;
    cin >> n >> m;
    
    int** matrix = createMatrix(n, m);
    
    // четене
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> matrix[i][j];
        }
    }
    
    // ... работа с матрицата ...
    
    deleteMatrix(matrix, n);
    return 0;
}
```

### Динамичен 3D Масив

```cpp
int*** create3D(int n, int m, int q) {
    int*** arr = new int**[n];
    for (int i = 0; i < n; i++) {
        arr[i] = new int*[m];
        for (int j = 0; j < m; j++) {
            arr[i][j] = new int[q];
        }
    }
    return arr;
}

void delete3D(int*** arr, int n, int m) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            delete[] arr[i][j];
        }
        delete[] arr[i];
    }
    delete[] arr;
}
```

### Memory Leak Prevention

```cpp
// ❌ ГРЕШКА - Memory leak!
void bad() {
    int* p = new int[100];
    // забравено delete[]
} // паметта никога не се освобождава

// ✅ ПРАВИЛНО
void good() {
    int* p = new int[100];
    // ... работа ...
    delete[] p;
}
```

---

## 👆 Указатели (Pointers)

### Основи

```cpp
int x = 5;
int* ptr = &x;      // ptr съдържа адреса на x

cout << ptr;        // адрес (напр. 0x7fff...)
cout << *ptr;       // стойност (5) - дереференция
cout << &ptr;       // адрес на самия указател

*ptr = 10;          // x става 10
```

### Указатели и Масиви

```cpp
int arr[] = {1, 2, 3, 4, 5};
int* p = arr;       // p сочи към първия елемент

// Еквивалентни:
arr[i]   <==>   *(arr + i)
&arr[i]  <==>   arr + i

// Pointer arithmetic
p++;                // p сочи към arr[1]
p += 2;             // p сочи към arr[3]

cout << p[0];       // 4 (arr[3])
cout << p[-1];      // 3 (arr[2])
```

### Const Pointers (ВАЖНО!)

```cpp
// 1. Pointer to const - НЕ може да промениш стойността
const int* p1 = &x;
// int const* p1 = &x;  // същото
*p1 = 5;            // ❌ ГРЕШКА
p1 = &y;            // ✅ OK

// 2. Const pointer - НЕ може да промениш къде сочи
int* const p2 = &x;
*p2 = 5;            // ✅ OK
p2 = &y;            // ❌ ГРЕШКА

// 3. Const pointer to const - нищо не може да промениш
const int* const p3 = &x;
*p3 = 5;            // ❌ ГРЕШКА
p3 = &y;            // ❌ ГРЕШКА
```

### nullptr

```cpp
int* p = nullptr;   // безопасна инициализация

// ВИНАГИ проверявай преди дереференция!
if (p != nullptr) {
    cout << *p;
}

// Или по-кратко:
if (p) {
    cout << *p;
}
```

---

## 📝 Символни Низове (C-Strings)

### Основи

```cpp
// Различни начини за създаване
char str1[] = "Hello";              // размер = 6 (5 + '\0')
char str2[10] = "Hello";            // размер = 10, остатъка = '\0'
char str3[] = {'H', 'e', 'l', 'l', 'o', '\0'};

// ГРЕШКА - няма място за '\0'
char bad[5] = "Hello";              // ❌ 

// Динамично
char* str = new char[100];
// ... работа ...
delete[] str;
```

### Четене от Конзола

```cpp
char buffer[1024];

// cin >> спира на whitespace
cin >> buffer;                      // "Hello World" -> "Hello"

// cin.getline чете целия ред
cin.getline(buffer, 1024);          // "Hello World" -> "Hello World"

// ВНИМАНИЕ: след cin >> остава '\n' в потока!
int n;
cin >> n;
cin.ignore();                       // изчисти '\n'
cin.getline(buffer, 1024);
```

### Основни Операции

```cpp
// ========== Копиране на подстринг ==========
char* substring(const char* str, int beg, int end) {
    size_t len = myStrLen(str);
    if (beg >= len) {
        char* empty = new char[1];
        empty[0] = '\0';
        return empty;
    }
    if (end > len) end = len;
    
    int newLen = end - beg;
    char* result = new char[newLen + 1];
    for (int i = 0; i < newLen; i++) {
        result[i] = str[beg + i];
    }
    result[newLen] = '\0';
    return result;
}

// ========== Филтриране на символи ==========
char* filterLowerCase(const char* str) {
    size_t len = myStrLen(str);
    
    // Първо преброй
    size_t count = 0;
    for (size_t i = 0; i < len; i++) {
        if (isLower(str[i])) count++;
    }
    
    // После създай с ТОЧНА ГОЛЕМИНА
    char* result = new char[count + 1];
    size_t j = 0;
    for (size_t i = 0; i < len; i++) {
        if (isLower(str[i])) {
            result[j++] = str[i];
        }
    }
    result[j] = '\0';
    return result;
}

// ========== Цензуриране на цифри ==========
char* censorDigits(const char* str) {
    size_t len = myStrLen(str);
    char* result = new char[len + 1];
    
    for (size_t i = 0; i < len; i++) {
        result[i] = isDigit(str[i]) ? '*' : str[i];
    }
    result[len] = '\0';
    return result;
}

// ========== Брой срещания на символ ==========
int countChar(const char* str, char c) {
    int count = 0;
    for (size_t i = 0; str[i]; i++) {
        if (str[i] == c) count++;
    }
    return count;
}

// ========== Брой срещания на подстринг ==========
int countSubstring(const char* text, const char* pattern) {
    int count = 0;
    size_t textLen = myStrLen(text);
    size_t patLen = myStrLen(pattern);
    
    for (size_t i = 0; i + patLen <= textLen; i++) {
        bool match = true;
        for (size_t j = 0; j < patLen; j++) {
            if (text[i + j] != pattern[j]) {
                match = false;
                break;
            }
        }
        if (match) count++;
    }
    return count;
}

// ========== Replace (in-place когато what <= where) ==========
void replace(char* text, const char* where, const char* what) {
    char result[MAX_SIZE];
    size_t textLen = myStrLen(text);
    size_t whereLen = myStrLen(where);
    size_t whatLen = myStrLen(what);
    size_t read = 0, write = 0;
    
    while (text[read]) {
        bool found = true;
        if (read + whereLen > textLen) {
            found = false;
        } else {
            for (size_t i = 0; i < whereLen; i++) {
                if (text[read + i] != where[i]) {
                    found = false;
                    break;
                }
            }
        }
        
        if (found) {
            for (size_t i = 0; i < whatLen; i++) {
                result[write++] = what[i];
            }
            read += whereLen;
        } else {
            result[write++] = text[read++];
        }
    }
    result[write] = '\0';
    myStrCpy(text, result);
}

// ========== Най-дълъг общ префикс ==========
void longestCommonPrefix(const char* s1, const char* s2, const char* s3, char* result) {
    size_t i = 0;
    while (s1[i] && s2[i] && s3[i] && 
           s1[i] == s2[i] && s2[i] == s3[i]) {
        result[i] = s1[i];
        i++;
    }
    result[i] = '\0';
}

// ========== Премахване на повтарящи се букви ==========
char* removeDuplicates(const char* str) {
    bool seen[26] = {false};
    size_t len = myStrLen(str);
    char* result = new char[len + 1];
    size_t j = 0;
    
    for (size_t i = 0; i < len; i++) {
        int idx = str[i] - 'a';
        if (!seen[idx]) {
            seen[idx] = true;
            result[j++] = str[i];
        }
    }
    result[j] = '\0';
    return result;
}

// ========== Броене на думи ==========
size_t countWords(const char* str) {
    size_t count = 0;
    bool inWord = false;
    
    for (size_t i = 0; str[i]; i++) {
        if (!isWhitespace(str[i]) && !isPunctuation(str[i])) {
            if (!inWord) {
                count++;
                inWord = true;
            }
        } else {
            inWord = false;
        }
    }
    return count;
}

// ========== Split на думи ==========
const size_t MAX_WORDS = 100;
const size_t MAX_WORD_LEN = 100;

void split(const char* str, char words[MAX_WORDS][MAX_WORD_LEN], size_t& wordsCount) {
    wordsCount = 0;
    bool inWord = false;
    size_t start = 0;
    
    for (size_t i = 0; ; i++) {
        bool isDelim = str[i] == '\0' || isWhitespace(str[i]) || isPunctuation(str[i]);
        
        if (!inWord && !isDelim) {
            start = i;
            inWord = true;
        } else if (inWord && isDelim) {
            size_t len = i - start;
            for (size_t j = 0; j < len; j++) {
                words[wordsCount][j] = str[start + j];
            }
            words[wordsCount][len] = '\0';
            wordsCount++;
            inWord = false;
        }
        
        if (str[i] == '\0') break;
    }
}
```

---

## 📊 Масиви и Матрици

### Основни Операции с Масиви

```cpp
// ========== Принтиране ==========
void printArray(const int* arr, int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

// ========== Min/Max ==========
int findMin(const int* arr, int size) {
    int min = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] < min) min = arr[i];
    }
    return min;
}

int findMax(const int* arr, int size) {
    int max = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] > max) max = arr[i];
    }
    return max;
}

// ========== Сума ==========
int sum(const int* arr, int size) {
    int s = 0;
    for (int i = 0; i < size; i++) {
        s += arr[i];
    }
    return s;
}

// ========== Средна стойност ==========
double average(const int* arr, int size) {
    return (double)sum(arr, size) / size;
}

// ========== Обръщане (reverse) ==========
void reverse(int* arr, int size) {
    for (int i = 0; i < size / 2; i++) {
        swap(arr[i], arr[size - 1 - i]);
    }
}

// ========== Линейно търсене ==========
int linearSearch(const int* arr, int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}

// ========== Проверка за симетричност ==========
bool isSymmetric(const int* arr, int size) {
    for (int i = 0; i < size / 2; i++) {
        if (arr[i] != arr[size - 1 - i]) return false;
    }
    return true;
}

// ========== Премахване на елемент по индекс ==========
void removeAt(int* arr, int& size, int index) {
    for (int i = index; i < size - 1; i++) {
        arr[i] = arr[i + 1];
    }
    size--;
}

// ========== Merge на два сортирани масива ==========
int* mergeSorted(const int* arr1, int size1, const int* arr2, int size2) {
    int* result = new int[size1 + size2];
    int i = 0, j = 0, k = 0;
    
    while (i < size1 && j < size2) {
        if (arr1[i] <= arr2[j]) {
            result[k++] = arr1[i++];
        } else {
            result[k++] = arr2[j++];
        }
    }
    
    while (i < size1) result[k++] = arr1[i++];
    while (j < size2) result[k++] = arr2[j++];
    
    return result;
}

// ========== Partition (около pivot) ==========
void partition(const int* arr, int size, int pivot, int* result, int& resultSize) {
    resultSize = size;
    int left = 0, right = size - 1;
    
    // По-малките наляво
    for (int i = 0; i < size; i++) {
        if (arr[i] < pivot) result[left++] = arr[i];
    }
    
    // Pivot-а
    for (int i = 0; i < size; i++) {
        if (arr[i] == pivot) result[left++] = arr[i];
    }
    
    // По-големите надясно (в обратен ред за да запазим реда)
    for (int i = size - 1; i >= 0; i--) {
        if (arr[i] > pivot) result[right--] = arr[i];
    }
}

// ========== Всички подмасиви ==========
void printAllSubarrays(const int* arr, int size) {
    for (int len = 1; len <= size; len++) {
        for (int start = 0; start + len <= size; start++) {
            cout << "[";
            for (int i = start; i < start + len; i++) {
                cout << arr[i];
                if (i < start + len - 1) cout << ", ";
            }
            cout << "] ";
        }
    }
    cout << endl;
}
```

### Матрици (2D Масиви)

```cpp
// ========== Принтиране ==========
void printMatrix(int** matrix, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }
}

// ========== Сравняване на матрици ==========
bool areEqual(int** m1, int** m2, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (m1[i][j] != m2[i][j]) return false;
        }
    }
    return true;
}

// ========== Транспониране (NxM -> MxN) ==========
int** transpose(int** matrix, int rows, int cols) {
    int** result = createMatrix(cols, rows);
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            result[j][i] = matrix[i][j];
        }
    }
    return result;
}

// ========== Матрично умножение (NxM * MxP = NxP) ==========
int** multiply(int** a, int** b, int n, int m, int p) {
    int** result = createMatrix(n, p);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < p; j++) {
            result[i][j] = 0;
            for (int k = 0; k < m; k++) {
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    return result;
}

// ========== Сума над главен диагонал ==========
int sumAboveDiagonal(int** matrix, int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            sum += matrix[i][j];
        }
    }
    return sum;
}

// ========== Сума под главен диагонал ==========
int sumBelowDiagonal(int** matrix, int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            sum += matrix[i][j];
        }
    }
    return sum;
}
```

---

## ⚡ Побитови Операции

### Основни Оператори

```cpp
// & (AND)   - 1 само ако и двата са 1
// | (OR)    - 1 ако поне един е 1
// ^ (XOR)   - 1 ако са различни
// ~ (NOT)   - обръща всички битове
// << (left shift)  - умножава по 2^n
// >> (right shift) - дели на 2^n

// Примери:
5 & 3   // 101 & 011 = 001 = 1
5 | 3   // 101 | 011 = 111 = 7
5 ^ 3   // 101 ^ 011 = 110 = 6
~5      // ...11111010 (two's complement)
5 << 1  // 1010 = 10
5 >> 1  // 10 = 2
```

### Полезни Битови Операции

```cpp
// ========== Проверка дали е четно ==========
bool isEven(int n) {
    return (n & 1) == 0;
}

// ========== 2^k ==========
int powerOf2(int k) {
    return 1 << k;
}

// ========== Вземи бит на позиция k ==========
int getBit(int n, int k) {
    return (n >> k) & 1;
}

// ========== Сложи бит на 1 на позиция k ==========
int setBit(int n, int k) {
    return n | (1 << k);
}

// ========== Сложи бит на 0 на позиция k ==========
int clearBit(int n, int k) {
    return n & ~(1 << k);
}

// ========== Обърни бит на позиция k ==========
int toggleBit(int n, int k) {
    return n ^ (1 << k);
}

// ========== Сложи стойност на бит ==========
int setBitValue(int n, int k, int value) {
    if (value) return setBit(n, k);
    else return clearBit(n, k);
}

// ========== Брой единици ==========
int countOnes(int n) {
    int count = 0;
    while (n) {
        count += n & 1;
        n >>= 1;
    }
    return count;
}

// ========== Обърни най-десната единица ==========
int clearRightmostOne(int n) {
    return n & (n - 1);
}

// ========== Последните k бита ==========
int getLastKBits(int n, int k) {
    return n & ((1 << k) - 1);
}

// ========== k бита от позиция m ==========
int getBitsFromPosition(int x, int m, int k) {
    return (x >> (m - k + 1)) & ((1 << k) - 1);
}

// ========== Намери уникалния елемент (XOR trick) ==========
int findUnique(const int* arr, int size) {
    int result = 0;
    for (int i = 0; i < size; i++) {
        result ^= arr[i];
    }
    return result;
}

// ========== Проверка дали k е част от двоичния запис на n ==========
bool isBinarySubstring(int n, int k) {
    if (k == 0) return true;
    
    // Намери броя битове на k
    int kBits = 0;
    int temp = k;
    while (temp) {
        kBits++;
        temp >>= 1;
    }
    
    // Провери всички възможни позиции
    int mask = (1 << kBits) - 1;
    while (n >= k) {
        if ((n & mask) == k) return true;
        n >>= 1;
    }
    return false;
}

// ========== Принтиране на всички подмножества ==========
void printAllSubsets(const int* arr, int size) {
    int total = 1 << size;  // 2^size
    
    for (int mask = 0; mask < total; mask++) {
        cout << "[";
        bool first = true;
        for (int i = 0; i < size; i++) {
            if (mask & (1 << i)) {
                if (!first) cout << ", ";
                cout << arr[i];
                first = false;
            }
        }
        cout << "] ";
    }
    cout << endl;
}
```

### Конверсии между Бройни Системи

```cpp
// ========== Десетично -> произволна система ==========
void toBase(int n, int base, char* result) {
    if (n == 0) {
        result[0] = '0';
        result[1] = '\0';
        return;
    }
    
    char digits[] = "0123456789ABCDEF";
    char temp[64];
    int i = 0;
    
    bool negative = n < 0;
    if (negative) n = -n;
    
    while (n > 0) {
        temp[i++] = digits[n % base];
        n /= base;
    }
    
    int j = 0;
    if (negative) result[j++] = '-';
    while (i > 0) {
        result[j++] = temp[--i];
    }
    result[j] = '\0';
}

// ========== Произволна система -> десетично ==========
int fromBase(const char* str, int base) {
    int result = 0;
    int sign = 1;
    int i = 0;
    
    if (str[0] == '-') {
        sign = -1;
        i = 1;
    }
    
    while (str[i]) {
        int digit;
        if (str[i] >= '0' && str[i] <= '9') {
            digit = str[i] - '0';
        } else if (str[i] >= 'A' && str[i] <= 'F') {
            digit = str[i] - 'A' + 10;
        } else if (str[i] >= 'a' && str[i] <= 'f') {
            digit = str[i] - 'a' + 10;
        }
        result = result * base + digit;
        i++;
    }
    
    return result * sign;
}
```

---

## 🔄 Сортиране и Търсене

### Bubble Sort

```cpp
void bubbleSort(int* arr, int n) {
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;  // Оптимизация!
    }
}
```

### Selection Sort

```cpp
void selectionSort(int* arr, int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }
}
```

### Binary Search (САМО за сортирани масиви!)

```cpp
int binarySearch(const int* arr, int size, int target) {
    int left = 0, right = size - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;  // не е намерен
}
```

### Counting Sort (за числа в интервал)

```cpp
// За числа в интервал [0, maxVal]
void countingSort(int* arr, int n, int maxVal) {
    int* count = new int[maxVal + 1]();
    
    for (int i = 0; i < n; i++) {
        count[arr[i]]++;
    }
    
    int idx = 0;
    for (int i = 0; i <= maxVal; i++) {
        while (count[i] > 0) {
            arr[idx++] = i;
            count[i]--;
        }
    }
    
    delete[] count;
}

// За числа в интервал [minVal, maxVal]
void countingSortRange(int* arr, int n, int minVal, int maxVal) {
    int range = maxVal - minVal + 1;
    int* count = new int[range]();
    
    for (int i = 0; i < n; i++) {
        count[arr[i] - minVal]++;
    }
    
    int idx = 0;
    for (int i = 0; i < range; i++) {
        while (count[i] > 0) {
            arr[idx++] = i + minVal;
            count[i]--;
        }
    }
    
    delete[] count;
}
```

---

## 🔢 Числови Операции

### Всички Числови Функции

```cpp
// ========== Абсолютна стойност ==========
int abs(int n) {
    return n < 0 ? -n : n;
}

// ========== Проверка за просто ==========
bool isPrime(int n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return false;
    }
    return true;
}

// ========== GCD (Евклид) ==========
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// ========== LCM ==========
int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}

// ========== Факторизация ==========
void printFactorization(int n) {
    for (int i = 2; i * i <= n; i++) {
        int count = 0;
        while (n % i == 0) {
            count++;
            n /= i;
        }
        if (count > 0) {
            cout << i << "^" << count << " ";
        }
    }
    if (n > 1) {
        cout << n << "^1";
    }
    cout << endl;
}

// ========== Palindrome ==========
bool isPalindrome(int n) {
    if (n < 0) return false;
    return n == reverseNumber(n);
}

// ========== Суфикс/Префикс/Инфикс ==========
bool isSuffix(int n, int k) {
    while (k > 0) {
        if (n % 10 != k % 10) return false;
        n /= 10;
        k /= 10;
    }
    return true;
}

bool isPrefix(int n, int k) {
    int nDigits = countDigits(n);
    int kDigits = countDigits(k);
    
    // Намали n до същия брой цифри като k
    for (int i = 0; i < nDigits - kDigits; i++) {
        n /= 10;
    }
    return n == k;
}

bool isInfix(int n, int k) {
    int kDigits = countDigits(k);
    int divisor = 1;
    for (int i = 0; i < kDigits; i++) divisor *= 10;
    
    while (n >= k) {
        if (n % divisor == k) return true;
        n /= 10;
    }
    return false;
}

// ========== Конкатенация на числа ==========
int concatenate(int a, int b) {
    int temp = b;
    while (temp > 0) {
        a *= 10;
        temp /= 10;
    }
    return a + b;
}

// ========== Най-често срещана цифра ==========
int mostFrequentDigit(int n) {
    if (n < 0) n = -n;
    int count[10] = {0};
    
    while (n > 0) {
        count[n % 10]++;
        n /= 10;
    }
    
    int maxCount = 0, result = 0;
    for (int i = 0; i < 10; i++) {
        if (count[i] > maxCount) {
            maxCount = count[i];
            result = i;
        }
    }
    return result;
}

// ========== Сортиране на цифрите ==========
int sortDigits(int n) {
    int count[10] = {0};
    bool negative = n < 0;
    if (negative) n = -n;
    
    while (n > 0) {
        count[n % 10]++;
        n /= 10;
    }
    
    int result = 0;
    for (int i = 1; i < 10; i++) {  // започваме от 1 за да няма водещи нули
        while (count[i] > 0) {
            result = result * 10 + i;
            count[i]--;
        }
    }
    // Добави нулите накрая
    while (count[0] > 0) {
        result *= 10;
        count[0]--;
    }
    
    return negative ? -result : result;
}
```

---

## ✅ Готови Решения

### Задача: Стринг само с малки букви

```cpp
char* filterLower(const char* str) {
    size_t count = 0;
    for (size_t i = 0; str[i]; i++) {
        if (isLower(str[i])) count++;
    }
    
    char* result = new char[count + 1];
    size_t j = 0;
    for (size_t i = 0; str[i]; i++) {
        if (isLower(str[i])) result[j++] = str[i];
    }
    result[j] = '\0';
    return result;
}
```

### Задача: Подстринг от beg до end

```cpp
char* substring(const char* str, int beg, int end) {
    size_t len = myStrLen(str);
    
    if (beg >= len) {
        char* empty = new char[1];
        empty[0] = '\0';
        return empty;
    }
    
    if (end > len) end = len;
    
    int newLen = end - beg;
    char* result = new char[newLen + 1];
    for (int i = 0; i < newLen; i++) {
        result[i] = str[beg + i];
    }
    result[newLen] = '\0';
    return result;
}
```

### Задача: Замяна на всяко четно/нечетно срещане

```cpp
void replaceOccurrences(char* str, char x, char odd, char even) {
    int count = 0;
    for (size_t i = 0; str[i]; i++) {
        if (str[i] == x) {
            count++;
            str[i] = (count % 2 == 1) ? odd : even;
        }
    }
}
```

### Задача: Лексикографско сравнение

```cpp
int lexCompare(const char* s1, const char* s2) {
    size_t i = 0;
    while (s1[i] && s2[i] && s1[i] == s2[i]) i++;
    
    if (s1[i] < s2[i]) return -1;
    if (s1[i] > s2[i]) return 1;
    return 0;
}
```

### Задача: Цензуриране с * (case-insensitive)

```cpp
void censorWord(char* text, const char* word) {
    size_t textLen = myStrLen(text);
    size_t wordLen = myStrLen(word);
    
    for (size_t i = 0; i + wordLen <= textLen; i++) {
        bool match = true;
        for (size_t j = 0; j < wordLen; j++) {
            if (toLower(text[i + j]) != toLower(word[j])) {
                match = false;
                break;
            }
        }
        if (match) {
            for (size_t j = 0; j < wordLen; j++) {
                text[i + j] = '*';
            }
        }
    }
}
```

### Задача: Разделяне на малки и главни букви

```cpp
void splitByCase(const char* str, char*& lower, char*& upper) {
    size_t lowerCount = 0, upperCount = 0;
    
    for (size_t i = 0; str[i]; i++) {
        if (isLower(str[i])) lowerCount++;
        else if (isUpper(str[i])) upperCount++;
    }
    
    lower = new char[lowerCount + 1];
    upper = new char[upperCount + 1];
    
    size_t li = 0, ui = 0;
    for (size_t i = 0; str[i]; i++) {
        if (isLower(str[i])) lower[li++] = str[i];
        else if (isUpper(str[i])) upper[ui++] = str[i];
    }
    lower[li] = '\0';
    upper[ui] = '\0';
}
```

### Задача: Jagged Array (редове с различна дължина)

```cpp
// Конкатенация на редове: първи + последен, втори + предпоследен...
int** concatenateRows(int** matrix, int* rowSizes, int rows, int* newSizes) {
    int** result = new int*[rows];
    
    for (int i = 0; i < rows; i++) {
        int j = rows - 1 - i;
        newSizes[i] = rowSizes[i] + rowSizes[j];
        result[i] = new int[newSizes[i]];
        
        int idx = 0;
        // Копирай последния ред
        for (int k = 0; k < rowSizes[j]; k++) {
            result[i][idx++] = matrix[j][k];
        }
        // Копирай първия ред
        for (int k = 0; k < rowSizes[i]; k++) {
            result[i][idx++] = matrix[i][k];
        }
    }
    
    return result;
}
```

---

## 📝 Възможни Изпитни Задачи

### Категория: Стрингове

1. **Филтриране** - само малки/главни/цифри/букви
2. **Подстринг** - от индекс beg до end
3. **Замяна** - на символ/дума/pattern
4. **Броене** - символи/думи/срещания
5. **Сравнение** - лексикографско
6. **Премахване** - повтарящи се символи
7. **Split** - на думи
8. **Обръщане** - на стринг/думи
9. **Longest Common Prefix**
10. **atoi/itoa** - конверсия число<->стринг

### Категория: Масиви

1. **Merge** - на два сортирани масива
2. **Partition** - около pivot
3. **Всички подмасиви**
4. **Симетричност**
5. **Най-дълга последователност** (намаляваща/еднакви)
6. **Обединение/Сечение** на множества
7. **Подмасив** - проверка

### Категория: Матрици

1. **Транспониране**
2. **Умножение**
3. **Сума над/под диагонал**
4. **Jagged arrays** - различни дължини на редовете
5. **Сравняване**

### Категория: Битове

1. **Брой единици**
2. **Последни k бита**
3. **k бита от позиция m**
4. **Намиране на уникален** (XOR)
5. **Всички подмножества**
6. **Конверсия между бройни системи**

### Категория: Числа

1. **Обръщане**
2. **Palindrome**
3. **Просто число**
4. **GCD/LCM**
5. **Факторизация**
6. **Най-често срещана цифра**
7. **Сортиране на цифри**
8. **Prefix/Suffix/Infix**

---

## ⚠️ Чести Грешки - НЕ ГИ ПРАВИ!

```cpp
// ❌ Забравена терминираща нула
char result[10];
result[0] = 'H';
// result няма '\0' -> UB!

// ✅ Правилно
result[1] = '\0';

// ❌ Забравен delete[]
int* arr = new int[100];
// ... край на функция без delete[]

// ✅ Правилно
delete[] arr;

// ❌ Използване на delete вместо delete[]
int* arr = new int[100];
delete arr;  // ГРЕШКА!

// ✅ Правилно
delete[] arr;

// ❌ Out of bounds
int arr[5];
arr[5] = 10;  // ГРЕШКА! Индексите са 0-4

// ❌ Размер на масива в функция
void f(int arr[]) {
    int size = sizeof(arr);  // ГРЕШКА! Това е sizeof(int*)
}

// ✅ Правилно - подай размера като параметър
void f(int arr[], int size) { ... }

// ❌ Непроверен nullptr
void f(char* str) {
    cout << str[0];  // Crash ако str е nullptr!
}

// ✅ Правилно
void f(char* str) {
    if (!str) return;
    cout << str[0];
}
```

---

## 🎯 Quick Reference Card

| Операция | Код |
|----------|-----|
| Дължина на стринг | `while (str[len]) len++;` |
| Копиране | `while (src[i]) dest[i] = src[i++]; dest[i] = '\0';` |
| Малка буква? | `c >= 'a' && c <= 'z'` |
| Цифра? | `c >= '0' && c <= '9'` |
| toLower | `c + ('a' - 'A')` |
| char -> digit | `c - '0'` |
| digit -> char | `'0' + d` |
| Четно? (битово) | `(n & 1) == 0` |
| 2^k | `1 << k` |
| k-ти бит | `(n >> k) & 1` |
| Последни k бита | `n & ((1 << k) - 1)` |
| XOR trick | `a ^ a = 0`, `a ^ 0 = a` |
| Брой цифри | `while (n) { count++; n /= 10; }` |
| k-та цифра | `(n / 10^k) % 10` |
| Обърни число | `rev = rev * 10 + n % 10; n /= 10;` |

---

## 🏆 Final Tips

1. **Първо прочети ЦЯЛАТА задача** - разбери какво се иска
2. **Идентифицирай типа** - стринг? масив? битове? число?
3. **Планирай структурата** - какви функции ще ти трябват?
4. **Валидирай входа** - `if (!ptr) return;`
5. **Терминирай стринговете** - `str[i] = '\0';`
6. **Освободи паметта** - `delete[]`
7. **Тествай с edge cases** - празен вход, 0, отрицателни
8. **Не усложнявай** - най-простото решение е най-доброто

---

**Успех на изпита! 💪**
