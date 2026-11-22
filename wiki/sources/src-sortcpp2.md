# 來源摘要：cppreference `std::sort` 官方文件

- **原始檔案**：`raw/problems/sortcpp2.md`
- **Ingest 日期**：2026-11-22
- **核心貢獻**：
  1. 定義 `<algorithm>` 標頭檔與 `[first, last)` 前閉後開迭代器區間。
  2. 釐清比較器 `comp(a, b)` 的本質：`a` 排在 `b` 前面時回傳 `true`。
  3. 確立最壞與平均時間複雜度皆為 $O(N \log N)$（底層採用 Introsort）。
  4. 提供 4 種比較器寫法（預設 `<`、`std::greater`、Functor、Lambda）。
- **關聯概念頁**：[[std-sort-basics]]、[[std-sort-custom-cmp]]