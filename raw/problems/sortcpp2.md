# 原始資料 02：std::sort 官方技術文件摘要 (cppreference)

- **來源**：cppreference.com (`std::sort`)
- **標頭檔**：`<algorithm>`
- **核心功能**：對目標範圍 `[first, last)` 內的元素進行排序（預設由小到大，不保證相等元素的原始相對順序）。

## 1. 常用函式原型 (Function Prototypes)
1. **預設排序**（使用 `<` 運算子，由小到大）：
   `void sort( RandomIt first, RandomIt last );`
2. **自訂規則排序**（傳入比較器 `comp`）：
   `void sort( RandomIt first, RandomIt last, Compare comp );`
*(註：C++17 起另有支援並行運算 `ExecutionPolicy` 的重載版本。)*

## 2. 關鍵參數與要求 (Parameters & Requirements)
- **`first, last`**：定義排序範圍的一對迭代器（前閉後開區間 `[first, last)`）。必須支援隨機存取（RandomAccessIterator，如 `vector`、`array` 或原生指標）。
- **`comp` (比較函式)**：
  - 簽名形式：`bool cmp(const Type1& a, const Type2& b);`
  - **核心邏輯**：當「第一個參數 `a` 應該排在第二個參數 `b` 前面」時，回傳 `true`；否則回傳 `false`。
  - **限制**：函式內部不可修改傳入的物件（建議加 `const&`）。

## 3. 時間複雜度與底層實作 (Complexity & Implementation)
- **時間複雜度**：保證為 **$O(N \log N)$** 次比較（其中 $N = \text{last} - \text{first}$）。
- **底層演算法**：通常採用 **內省排序 (Introsort)**——結合快速排序 (Quicksort)、堆積排序 (Heapsort) 與插入排序 (Insertion Sort)，解決了傳統 Quicksort 在最壞情況下退化成 $O(N^2)$ 的問題（自 C++98 缺陷報告 LWG 713 修正後強制要求）。

## 4. 四種常見寫法範例 (Code Examples)
```cpp
#include <algorithm>
#include <array>
#include <functional>

std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

// 1. 預設由小到大 (operator<)
std::sort(s.begin(), s.end()); 

// 2. 標準函式庫內建由大到小 (std::greater)
std::sort(s.begin(), s.end(), std::greater<int>()); 

// 3. 自訂函式物件 (Functor / Struct)
struct {
    bool operator()(int a, int b) const { return a < b; }
} customLess;
std::sort(s.begin(), s.end(), customLess);

// 4. Lambda 匿名函式 (C++11 起最常用寫法)
std::sort(s.begin(), s.end(), [](int a, int b) {
    return a > b; // 由大到小
});