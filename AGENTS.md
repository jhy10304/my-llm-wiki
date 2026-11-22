# AGENTS.md - 跨領域學習知識庫網站 (LLM Wiki & Static Web Schema)

## 1. 專案目的 (Project Purpose)
本專案結合 **LLM Wiki 持久化知識管理架構** 與 **靜態網頁展示系統**，用於系統化累積大學核心課程（初級會計學、計算機組織、作業系統、行銷管理）與 C++ 演算法解題筆記。
- **核心運作模式**：AI 負責讀取 `raw/` 內的唯讀原始題目與講義，將知識編譯為結構化的 `wiki/` Markdown 檔案，並同步生成與維護 `pages/` 下的高可讀性 HTML 網頁與 `index.html` 首頁導覽，讓知識不僅能跨文件連結，也能透過網頁隨時瀏覽複習。

## 2. 網頁整體風格與技術棧 (Visual Style & Tech Stack)
- **技術棧**：純靜態網頁（HTML5 + Vanilla CSS），零外部框架依賴，確保可直接在 GitHub Pages 穩定部署與快速載入。
- **整體風格**：現代技術文件風（Technical Documentation Style），採用高對比淺色系背景（`#F8FAFC`）搭配純白知識卡片（`#FFFFFF`），強調閱讀舒適度與結構清晰度。
- **領域色彩識別（Domain Color Coding）**：
  - **商管會計類（Accounting / Business）**：使用沉穩綠（`#059669`）作為分類標籤與重點邊框色。
  - **資工與演算法類（CS / Algorithms）**：使用科技藍（`#2563EB`）作為分類標籤與程式碼強調色。

## 3. 頁面結構 (Page Structure)
1. **首頁 (`index.html`)**：
   - **頂部標頭 (Header)**：網站標題、副標題說明（LLM Wiki 運作概念）與系統狀態徽章。
   - **知識庫統計與分類導覽**：依「核心課程」與「演算法與程式設計」分區展示知識卡片（每張卡片包含：領域標籤、頁面標題、一句話核心摘要、關聯連結標籤、閱讀內頁按鈕）。
   - **最近維護日誌 (Changelog Preview)**：同步顯示 `log.md` 的最新編譯紀錄，展現知識庫隨時間累積的軌跡。
2. **知識庫內頁 (`pages/*.html`)**：
   - **頂部麵包屑導覽**：提供「← 返回知識庫總目錄 (`../index.html`)」連結。
   - **文章標頭區**：分類徽章、主標題、來源檔案標註（對應 `raw/`）與最後更新日期。
   - **核心摘要框 (TL;DR Callout)**：置頂呈現 2～3 點核心結論。
   - **結構化內容區**：依領域需求呈現會計分錄借貸對齊表格，或附帶語法區塊的 C++ 程式碼與時間複雜度分析。
   - **關聯知識連結區 (Related Wiki Links)**：頁面底部列出與本篇相關的其他知識頁面連結，實現 Wiki 網狀導覽。

## 4. 檔案命名與目錄放置規範 (File Naming & Directory Structure)
所有檔案與資料夾一律使用 **全小寫英數字與連字號（kebab-case）**，嚴禁使用空格或中文字元作為檔名：

```text
/
├── AGENTS.md                           # [Schema] 本專案最高指導規範文件
├── index.html                          # [Web 首頁] 知識庫總覽與卡片導覽入口
├── index.md                            # [Wiki 索引] Markdown 版全站目錄
├── log.md                              # [Wiki 日誌] 知識庫編譯與更新時間軸
├── assets/
│   └── style.css                       # [樣式表] 全站共用 CSS 變數與卡片/表格排版
├── raw/                                # [Layer 1: 原始素材層 - 唯讀不可修改]
│   └── problems/
│       ├── bank-reconciliation-terrell.md
│       └── uva-11321.md
├── wiki/                               # [Layer 2: 知識編譯層 - Markdown 知識庫]
│   ├── concepts/
│   │   └── bank-reconciliation.md
│   └── entities/
│       └── std-sort-custom-cmp.md
└── pages/                              # [Layer 3: 網頁展示層 - 對應 Wiki 內容的 HTML]
    ├── bank-reconciliation.html
    └── std-sort-custom-cmp.html

## 教學知識庫與簡報生成規範 (2026-11-22 討論確認新增)

### 1. 概念頁 (`wiki/concepts/`) 強制教學模板
為確保後續能直接產出「初學者看得懂的教學型 HTML 簡報」，所有 `concepts/` 下的頁面除核心定義外，**必須**包含以下三個教學區塊：
- **💡 白話生活比喻 (Analogy)**：用日常情境解釋抽象概念，禁止堆砌術語。
- **💻 程式碼對照範例 (Code Comparison)**：提供可直接編譯的極簡 C++ 範例。
- **⚠️ 新手常見地雷 (Common Pitfalls)**：明確列出初學者最容易寫錯的語法或邏輯錯誤。

### 2. Ingest 增量合併與檢查規則
- 每次僅處理 **1 篇** `raw/problems/` 原始資料，嚴禁一次批次敷衍。
- 寫入 `concepts/` 時必須比對既有內容：合併重複概念、補充細節，若發現不同來源說法衝突須明確標註。
- 每次操作完畢，必須同步更新 `wiki/index.md` 與 `wiki/log.md`（格式：`## [YYYY-MM-DD] ingest | 來源名稱 - 異動摘要`）。

### 3. 簡報生成前健康檢查 (Lint Gate)
- 在執行 `Query` 產出 HTML 簡報前，必須先經過 `Lint` 檢查並建立 `wiki/syntheses/sort-tutorial-outline.md` 中介大綱，確認無未解釋之專有名詞後才可進行前端渲染。