# 原始資料 05：排序核心原理、穩定性與比較函數 (演算法筆記)

- **來源**：演算法筆記 (Algorithm Notes) — Sort 章節精華萃取
- **核心重點**：釐清比較式排序與非比較式排序的差異、字典序（Lexicographical Order）、穩定排序（Stable Sort），以及 C++ `std::sort` 比較函數的嚴格要求。

## 1. 排序的兩大流派與底層演進
1. **比較式排序（Comparison-based / 對調式排序）**：
   - 透過兩兩比較大小、交換位置來排序（不限於數字，只要能定義「誰在誰前面」的**全序關係 Total Order** 皆可排）。
   - **效率下限**：任何比較式排序演算法的比較次數下限已被證明為 $\Omega(N \log N)$。
   - **演進路線**：基礎為氣泡/選擇/插入排序 $O(N^2)$ $\rightarrow$ 快速排序 QuickSort（最壞 $O(N^2)$） $\rightarrow$ **Introsort**（C++ `std::sort` 標準實作，保證 $O(N \log N)$） $\rightarrow$ 現代改良版 `pdqsort` 與 Google DeepMind 透過強化學習改良組合語言指令的 **AlphaDev**。
2. **非比較式排序（Non-comparison-based / 放置式排序）**：
   - 如 Counting Sort（計數排序）、Radix Sort（基數排序），需額外記憶體開銷且僅限特定型別（如整數），但時間複雜度可突破 $O(N \log N)$ 達到線性 $O(N+R)$。

## 2. 什麼東西可以排序？（等價於數組與字典序）
- **單一鍵值（Key）**：字元（ASCII 碼）、記憶體指標、或是結構體中的某個特定欄位。
- **多維度數組（Tuple / Struct）與字典序（Lexicographical Order）**：
  - 規則：先比較第一維度；若平手，再比較第二維度；若又平手，再比較第三維度（如同英文字典排列單字的方式）。

## 3. 穩定排序（Stable Sort）的意義
- **定義**：鍵值（Key）相同的資料，原本排在前面的，排序後依然在前面；原本排在後面的，排序後依然在後面（**鍵值相同、相對順序不變**）。
- **C++ 函式庫對照**：
  - `std::sort()`：**不穩定排序**（底層為 Introsort，相等元素的相對順序可能被打亂）。
  - `std::stable_sort()`：**穩定排序**（底層通常為 Merge Sort 合併排序），當題目或業務邏輯要求「分數相同時，維持原本輸入/報名順序」時必須使用它。

## 4. C `qsort` vs. C++ `std::sort` 比較函數差異
- **C 語言 `qsort()`**：回傳 `int`（負數、零、正數分別代表 `<`、`=`、`>`）。
- **C++ `std::sort()`**：回傳 `bool`，語意為「嚴格小於（`<`）」：
  - 回傳 `true`：代表第一個參數 `a` 必須排在第二個參數 `b` 前面（`a < b`）。
  - 回傳 `false`：代表 `a` 不排在 `b` 前面（`a >= b`，包含相等的情況）。
  ```cpp
  bool cmp(const int& a, const int& b) {
      return a < b; // 嚴格小於，相等時必須回傳 false
  }