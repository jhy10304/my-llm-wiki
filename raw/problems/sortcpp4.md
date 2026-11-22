# 原始資料 04：常用 C++ algorithm：sort 速查筆記 (Yui Huang)

- **來源**：Yui Huang 演算法學習筆記（【筆記】常用C++ algorithm：sort）
- **核心重點**：整理傳統陣列 `array` 與動態陣列 `vector` 的 `std::sort` 呼叫語法差異、反向迭代器降序技巧，以及 `pair` 雙條件不同方向排序範例。

## 1. 傳統陣列 `a[]` vs. 動態陣列 `vector` 寫法對照
`std::sort` 皆為**就地排序（In-place）**，直接修改原陣列，無回傳值（`void`）。

### (1) 傳統陣列 `a[]`（長度為 $N$）
傳入的是記憶體指標起點與終點後一格：
```cpp
sort(a, a + N);       // 由小到大排序索引 [0, N-1]
sort(a, a + N, cmp);  // 搭配自訂 cmp 函式排序