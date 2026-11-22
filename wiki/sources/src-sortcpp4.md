# 來源摘要：常用 C++ algorithm：sort 速查筆記 (Yui Huang)

- **原始檔案**：`raw/problems/sortcpp4.md`
- **Ingest 日期**：2026-11-22
- **核心貢獻**：
  1. 明確區分傳統陣列 `a[]`（傳指標 `a, a + N`）與動態陣列 `vector`（傳迭代器 `v.begin(), v.end()`）的呼叫語法。
  2. 提供免寫 `cmp` 的反向迭代器降序技巧：`sort(v.rbegin(), v.rend())`。
  3. 提供 ZeroJudge c230 雙條件「一升一降」排序範例（第一欄升序、平手時第二欄降序）。
- **關聯概念頁**：[[std-sort-basics]]、[[std-sort-custom-cmp]]