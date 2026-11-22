---
title: std::sort 與自訂比較函式 (Custom Comparator)
category: algorithms
type: entity
tags: [C++, STL, std::sort, 時間複雜度]
sources_count: 1
updated_at: 2026-11-15
---

# std::sort 與自訂比較函式 (Custom Comparator)

`std::sort` 是 C++ `<algorithm>` 標頭檔中的內省排序（Introsort）函式，平均與最差時間複雜度皆為 $O(N \log N)$。

## 自訂比較函式撰寫規範
比較函式 `bool cmp(int a, int b)` 回傳 `true` 代表 **`a` 應該排在 `b` 前面**（嚴格弱序 Strict Weak Ordering，兩者相等時必須回傳 `false`）。

### 實作範例（應用於 [[src-uva-11321|UVa 11321]]）
```cpp
#include <algorithm>
#include <cstdlib>
using namespace std;

int M;

bool cmp(int a, int b) {
    // 1. 先比除以 M 的餘數（小到大）
    if (a % M != b % M) {
        return a % M < b % M;
    }
    // 2. 餘數相同，判斷奇偶性（注意負奇數取餘為 -1，故用 != 0 判斷）
    bool a_is_odd = (a % 2 != 0);
    bool b_is_odd = (b % 2 != 0);
    
    if (a_is_odd != b_is_odd) {
        return a_is_odd; // 奇數排在偶數前面
    }
    // 3. 同為奇數（大到小）或同為偶數（小到大）
    if (a_is_odd) {
        return a > b;
    }
    return a < b;
}
### 經典變體：第一條件升序、第二條件降序 (源自 [[src-sortcpp4]])
例如 ZeroJudge c230 題型，先依 `first` 由小到大排；若 `first` 相同，則依 `second` 由大到小排：
```cpp
#define pii pair<int, int>

bool cmp(pii a, pii b) {
    if (a.first == b.first) return a.second > b.second; // 平手時：第二欄由大到小 (>)
    return a.first < b.first;                           // 預設：第一欄由小到大 (<)
}