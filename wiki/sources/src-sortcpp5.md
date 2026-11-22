# 來源摘要：排序核心原理、穩定性與比較函數 (演算法筆記)

- **原始檔案**：`raw/problems/sortcpp5.md`
- **Ingest 日期**：2026-11-22
- **核心貢獻**：
  1. 釐清比較式排序（下限 $\Omega(N \log N)$）與非比較式排序（如 Counting Sort $O(N+R)$）的差異。
  2. 定義「字典序（Lexicographical Order）」多維度比較規則。
  3. 區分 `std::sort`（不穩定）與 `std::stable_sort`（穩定排序，保留相等元素原始相對順序）。
  4. 釐清 C++ `cmp` 回傳 `bool` 代表「嚴格小於 `<`」，相等時必須回傳 `false`（嚴格弱序 Strict Weak Ordering）。
- **關聯概念頁**：[[stable-sort-and-pitfalls]]、[[std-sort-custom-cmp]]