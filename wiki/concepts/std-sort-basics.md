# 概念頁：C++ `std::sort` 基礎語法與核心原理

- **標頭檔**：`#include <algorithm>`
- **時間複雜度**：平均與最壞皆為 $O(N \log N)$
- **底層實作**：Introsort（內省排序）

## 1. 核心定義與基本用法
`std::sort(first, last)` 用於對連續記憶體區間 `[first, last)` 內的元素進行**就地排序（In-place）**，預設由小到大排列，無回傳值。
### 底層黑科技：Introsort（三合一混合排序）
`std::sort` 之所以能同時兼顧「平均極快」與「最壞情況也不會變慢」，是因為它結合了三種演算法（源自 [[src-sortcpp3]]）：
1. **主力用 QuickSort（快速排序）**：平時速度最快。
2. **遞迴太深時切換 HeapSort（堆積排序）**：遇到極端測資時防止退化成 $O(N^2)$。
3. **資料量變小時切換 InsertionSort（插入排序）**：小區間直接插入，省去函式呼叫開銷。

### 進階技巧：只排序「部分區間」
不一定要整條陣列都排，可以透過迭代器加減偏移量，只整理中間一段：
```cpp
// 只排序索引 [1, n-2] 的元素，頭尾第一個與最後一個元素保持原位不動
sort(v.begin() + 1, v.end() - 1);

## 2. 💡 白話生活比喻 (Analogy)
- **前閉後開區間 `[first, last)`**：就像排隊買票時，老師點名「從**第 1 號同學**開始，到**第 6 號同學的前面一位**為止」，實際上只有 `1, 2, 3, 4, 5` 號會被整理，第 6 號（`end()`）是一個站在隊伍最尾巴後面的「空氣界線標誌」。
- **為什麼速度是 $O(N \log N)$**：想像整理 1,000 張考卷，如果一張一張找最小值（氣泡/選擇排序 $O(N^2)$）要花 100 萬次動作；`std::sort` 懂得「對半切開分頭整理」（Introsort），只需要約 10,000 次動作就能排完。

## 3. 💻 程式碼對照範例 (Code Comparison)
```cpp
#include <iostream>
#include <vector>
#include <algorithm> // 必須引入此標頭檔
using namespace std;

int main() {
    vector<int> v = {5, 2, 9, 1, 7};
    
    // 預設由小到大排序：[1, 2, 5, 7, 9]
    sort(v.begin(), v.end()); 
    
    // 使用內建比較器由大到小排序：[9, 7, 5, 2, 1]
    sort(v.begin(), v.end(), greater<int>()); 
    return 0;
}
### 3. 更新 `wiki/concepts/std-sort-custom-cmp.md`
打開你原本就有的 `wiki/concepts/std-sort-custom-cmp.md`，確保它符合我們新的 `AGENTS.md` 教學模板，並把 `sortcpp3` 的 **Lambda 寫法** 與 **`pair` 雙條件排序** 補進去（你可以直接附加在原本 UVa 11321 的內容後面）：

```markdown
## 💡 白話生活比喻 (Analogy)
- **`cmp(a, b)` 的本質**：就像比賽裁判，每次抓兩位選手 `a` 和 `b` 來問：「請問 **`a` 應該排在 `b` 前面嗎？**」
  - 回傳 `true`：「是！`a` 排在 `b` 前面。」
  - 回傳 `false`：「否！`a` 不用排在 `b` 前面（包含兩人平手的情況）。」
- **多條件排序**：就像學校發獎狀，先比「總分高到低」；如果總分平手，再比「國文分數高到低」。

## 💻 進階寫法：Lambda 匿名函式與雙條件排序
除了寫獨立的 `bool cmp(...)` 函式，現代 C++ 更常直接把規則寫在 `sort` 括號裡（Lambda 表達式），適合處理 `pair` 或多欄位結構（源自 [[src-sortcpp3]]）：

```cpp
// 範例：區間陣列 vector<pair<int, int>>
// 規則：先比「起點」由小到大；若起點相同，再比「終點」由小到大
sort(intervals.begin(), intervals.end(),
     [](const pair<int, int>& a, const pair<int, int>& b) {
         if (a.first == b.first) return a.second < b.second; // 起點平手，比終點
         return a.first < b.first;                           // 預設比起點
     });

---
### 傳統陣列 `a[]` vs. 動態陣列 `vector` 語法對照 (源自 [[src-sortcpp4]])
初學者常混用兩種陣列，傳入參數的方式不同：
```cpp
// 1. 傳統陣列 a[]（長度為 N）：傳入記憶體指標
int a[5] = {5, 2, 9, 1, 7};
sort(a, a + 5);          // 由小到大排序索引 [0, 4]
sort(a, a + 5, cmp);     // 搭配自訂規則

// 2. 動態陣列 vector：傳入容器迭代器
vector<int> v = {5, 2, 9, 1, 7};
sort(v.begin(), v.end());     // 由小到大
sort(v.rbegin(), v.rend());   // 小技巧：用反向迭代器直接由大到小（免寫 cmp！）

- **傳統陣列少加元素個數**：長度為 `N` 的陣列必須寫 `sort(a, a + N)`；若誤寫成 `sort(a, a + N - 1)`，最後一個元素會完全沒排到。