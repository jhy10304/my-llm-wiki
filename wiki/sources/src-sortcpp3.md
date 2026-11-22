# 來源摘要：C++ STL sort() 技巧與實戰應用 (iThome 鐵人賽)

- **原始檔案**：`raw/problems/sortcpp3.md`
- **Ingest 日期**：2026-11-22
- **核心貢獻**：
  1. 拆解 Introsort 三合一混合演算法原理（QuickSort + HeapSort + InsertionSort）。
  2. 補充「部分區間排序」寫法（如 `v.begin() + 1, v.end() - 1`）。
  3. 介紹 C++11 Lambda 匿名比較器與 `vector<pair<int, int>>` 雙條件排序。
  4. 提供 4 大演算法解題應用場景（缺失數字、雙指針匹配、區間合併、貪心區間排程）。
- **關聯概念頁**：[[std-sort-basics]]、[[std-sort-custom-cmp]]