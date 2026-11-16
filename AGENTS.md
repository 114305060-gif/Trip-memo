# AGENTS.md - Trip Memo 專案開發與內容維護指南

## 1. 專案概述與目的 (Project Overview & Purpose)
- **專案名稱**：Trip Memo（旅途手帳）
- **專案目的**：打造一個記錄台灣國內與國外旅行足跡的個人靜態網站。透過規範化的檔案結構與版面模板，讓 AI 助手能快速、一致地協助新增未來的旅行日誌，無需每次重新設計排版。
- **目標受眾**：專案作者本人及對旅行路線、景點筆記有興趣的讀者。

---

## 2. 設計風格與視覺規範 (Design System & Visual Guidelines)
本專案採用**「溫暖咖啡館 × 紙本手帳（Cafe & Paper Notebook）」**風格，強調閱讀舒適度與筆記質感。AI 在修改或新增任何頁面時，**必須嚴格沿用** `css/style.css` 中定義的 CSS 變數，禁止擅自引入高飽和度螢光色或改變整體配色：

- **紙張背景色 (`--bg-paper`)**：`#F7F3EB`（帶有微細方眼格紋的手帳紙質感）
- **卡片背景色 (`--bg-card`)**：`#FFFDF9`（溫暖米白色，搭配虛線邊框與紙膠帶裝飾）
- **主要文字色 (`--text-main`)**：`#3B322C`（深焙咖啡棕）
- **次要文字色 (`--text-muted`)**：`#7A6E65`（拿鐵灰棕色）
- **台灣國內標籤色 (`--accent-green`)**：`#3A5A40`（森林深綠色）
- **國外旅遊標籤色 (`--accent-brown`)**：`#9C6644`（焦糖暖棕色）
- **邊框與分隔線 (`--border-color`)**：`#DFD5C5`
- **字體設定**：標題使用 `Noto Serif TC`（襯線體，營造書寫與出版感），內文使用 `Noto Sans TC`（無襯線體，確保易讀性）。

---

## 3. 資料夾與檔案命名規則 (Directory Structure & Naming Conventions)
所有檔案必須依照以下結構放置，嚴禁將旅行文章直接放在根目錄：

```text
trip-memo/
├── AGENTS.md                 # 本專案的 AI 協作與開發規則文件
├── README.md                 # GitHub 專案說明文件
├── index.html                # 網站首頁（含分類篩選功能與所有旅行卡片列表）
├── css/
│   └── style.css             # 全站共用的手帳風樣式表
├── templates/
│   └── trip-template.html    # 新增旅行文章專用的標準 HTML 模板
└── trips/                    # 存放所有獨立旅行日誌頁面
    ├── tw-green-island.html  # 範例：台灣國內旅遊
    ├── tw-tainan.html        # 範例：台灣國內旅遊
    ├── intl-mongolia.html    # 範例：國外旅遊
    └── intl-singapore.html   # 範例：國外旅遊
```

### 檔案命名規則（一律使用小寫英文與連字號 `-`）：
- **台灣國內旅遊**：必須以 `tw-` 開頭，格式為 `tw-<地點英文名稱>.html`（例如：`tw-hualien.html`、`tw-jiufen.html`）。
- **國外旅遊**：必須以 `intl-` 開頭，格式為 `intl-<國家或城市英文名稱>.html`（例如：`intl-kyoto.html`、`intl-seoul.html`）。

---

## 4. 頁面架構與元素規範 (Page Structure & Component Rules)
1. **首頁 (`index.html`)**：
   - 包含網站標題區（Header）、分類篩選按鈕（全部 / 台灣國內 / 國外旅遊）、旅行卡片網格（`.trip-grid`）與頁尾（Footer）。
   - 每張旅行卡片（`<article class="trip-card">`）**必須包含** `data-category="taiwan"` 或 `data-category="overseas"` 屬性，以確保首頁的 JavaScript 分類篩選功能正常運作。
2. **旅行內頁 (`trips/*.html`)**：
   - 必須使用 `<link rel="stylesheet" href="../css/style.css">` 引入共用樣式（注意相對路徑為 `../`）。
   - 頁面結構依序為：返回首頁導覽連結 → 文章標頭（分類標籤、標題、旅遊日期與天數） → 行程亮點清單（Highlights） → 手帳隨筆心得（Memo Notes） → 頁尾。

---

## 5. 新增旅行紀錄的標準流程 (Workflow for Adding a New Trip)
當使用者要求 AI「新增一篇旅行紀錄」時，AI 必須依序執行以下 4 個步驟：

1. **確認分類與檔名**：
   - 判斷地點屬於「台灣國內（`taiwan` / 檔名前綴 `tw-`）」或「國外旅遊（`overseas` / 檔名前綴 `intl-`）」。
2. **複製模板建立新頁面**：
   - 完整複製 `templates/trip-template.html` 的結構，在 `trips/` 資料夾下建立新檔案。
   - 替換模板中的 `[分類標籤]`、`[地點標題]`、`[旅遊時間與天數]`、`[行程亮點]` 與 `[旅行隨筆內容]`。
3. **更新首頁卡片列表 (`index.html`)**：
   - 在 `index.html` 的 `<main class="trip-grid">` 區塊最上方（最新旅程優先），依照以下標準 HTML 片段插入新卡片：

```html
<article class="trip-card" data-category="taiwan或overseas">
  <div class="card-tape"></div>
  <div class="card-meta">
    <span class="badge badge-tw或badge-intl">台灣國內 或 國外旅遊</span>
    <span class="trip-date">YYYY.MM</span>
  </div>
  <h2 class="card-title"><a href="trips/檔案名稱.html">地點名稱｜副標題</a></h2>
  <p class="card-excerpt">50 字以內的簡短旅行印象摘要。</p>
  <div class="card-tags">
    <span>#標籤1</span>
    <span>#標籤2</span>
  </div>
  <a href="trips/檔案名稱.html" class="read-more">翻開手帳閱讀 →</a>
</article>
```

4. **檢查清單**：
   - 確認 `href` 路徑無誤、`data-category` 與 `badge` 顏色一致（台灣用 `taiwan` + `badge-tw`；國外用 `overseas` + `badge-intl`）。

---

## 6. AI 協作守則 (Rules for AI Assistants)
- **禁止修改核心樣式**：除非使用者明確要求全站改版，否則不要修改 `css/style.css` 或 `templates/trip-template.html`。
- **保持語言一致性**：網站介面與文章內容預設使用繁體中文，語氣保持溫暖、簡潔、有生活感。
- **不依賴外部圖片庫**：為避免日後圖片連結失效（破圖），版面設計以文字排版、Emoji 圖示、CSS 紙膠帶與標籤徽章為主；若使用者提供本地圖片，則統一放入 `images/` 資料夾並使用相對路徑引用。
