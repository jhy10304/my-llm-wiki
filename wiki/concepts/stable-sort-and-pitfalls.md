# 概念頁：穩定排序 (`stable_sort`) 與比較器致命地雷

- **關聯函式**：`std::sort` vs. `std::stable_sort`（皆定義於 `<algorithm>`）
- **核心原則**：嚴格弱序（Strict Weak Ordering）

## 1. 什麼是穩定排序（Stable Sort）？
當兩筆資料的「比較鍵值（Key）完全相同」時，排序後是否還能維持它們在原始輸入中的前後順序（源自 [[src-sortcpp5]]）：
- **`std::sort`（不穩定排序）**：底層為 Introsort，相等元素的原始順序**可能會被打亂**，但速度最快。
- **`std::stable_sort`（穩定排序）**：底層通常為 Merge Sort（合併排序），保證相等元素**原本在前面的，排完依然在前面**。

## 2. 💡 白話生活比喻 (Analogy)
- **穩定排序 (`stable_sort`)**：就像全班照「考試分數」從高到低排座位，如果小明和小華都考 90 分，老師規定「誰先交卷誰就坐前面」，不會隨便把兩人的順序對調。
- **嚴格弱序地雷（不能寫 `<=`）**：比較函式 `cmp(a, b)` 是在問「`a` 是否**嚴格贏過** `b`」。如果兩個人分數一模一樣（平手），裁判卻說「`a` 贏過 `b`（回傳 `true`）」，換個角度問又說「`b` 也贏過 `a`（回傳 `true`）」，裁判就會邏輯錯亂當機（程式發生 Segmentation Fault 崩潰）！

## 3. 💻 程式碼對照範例 (Code Comparison)
```cpp
#include <algorithm>
#include <vector>
#include <string>
using namespace std;

struct Student {
    string name;
    int score;
};

// ✅ 正確寫法：嚴格小於/大於（平手時必須回傳 false）
bool cmpScore(const Student& a, const Student& b) {
    return a.score > b.score; // 若寫成 >= 遇到相同分數會導致未定義行為甚至崩潰！
}

int main() {
    vector<Student> list = {{"Alice", 90}, {"Bob", 95}, {"Charlie", 90}};
    
    // 使用 stable_sort：Bob(95) 第一，Alice(90) 保證仍在 Charlie(90) 前面
    stable_sort(list.begin(), list.end(), cmpScore);
    return 0;
}