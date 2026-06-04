# RIITS 客戶簡報平台

這是一個專為客戶簡報設計的 GitHub Pages 站點。每份簡報都會保留成獨立 HTML deck，並用 Markdown 內容做成可複製的模板，方便後續持續新增、更新與下架。

- 首頁入口：`https://jerrysun1985-byte.github.io/RIITS/`
- 模板頁：`https://jerrysun1985-byte.github.io/RIITS/templates/deck/`（公開，不需密碼）
- Deck 路徑：`/RIITS/decks/<client>/<yyyy-mm-dd-topic-slug>/`
- 簡報索引：`/RIITS/data/decks.json`
- 密碼設定：`/RIITS/data/auth-config.json`

## 專案結構

- `index.html`: 首頁入口，顯示所有簡報卡片與篩選器
- `src/home.js`, `src/home.css`: 首頁渲染與視覺樣式
- `public/data/decks.json`: Deck 索引資料
- `public/data/auth-config.json`: Deck 密碼設定
- `public/templates/deck/`: 標準 deck 模板
- `public/decks/...`: 實際簡報頁
- `public/decks/riits/2026-06-04-image-table-template/`: 圖文表格正式範本
- `public/decks/<client>/<yyyy-mm-dd-topic-slug>/images/`: 圖片素材
- `public/decks/<client>/<yyyy-mm-dd-topic-slug>/assets/`: 其他附檔、圖表或匯出素材
- `public/assets/deck-loader.js`: reveal.js 啟動腳本
- `public/assets/auth-gate.js`: 密碼保護流程
- `scripts/sync-reveal-assets.mjs`: 將 npm 的 reveal.js 靜態資產同步到 `public/vendor`

## 新增簡報的標準流程

1. 複製模板

```bash
cp -R public/templates/deck public/decks/<client>/<yyyy-mm-dd-topic-slug>
```

2. 編輯 `public/decks/<client>/<yyyy-mm-dd-topic-slug>/slides.md`

3. 放入圖片或附件

```text
public/decks/<client>/<yyyy-mm-dd-topic-slug>/
  ├─ slides.md
  ├─ images/
  │  ├─ chart.png
  │  └─ photo.jpg
  └─ assets/
     └─ reference.pdf
```

在 `slides.md` 中用相對路徑引用，例如：

```md
![流程圖](./images/chart.png)
```

4. 更新 `public/data/decks.json`

必要欄位：

- `id`
- `client`
- `title`
- `date`
- `slug`
- `path`
- `tags`
- `status` (`active` | `archived` | `draft`)
- `auth.required`
- `auth.key`

5. 本機預覽與建置

```bash
npm run dev
npm run build
```

6. Push 到 `main`

GitHub Actions 會自動建置並發布到 Pages。

### 圖文表格範本

如果你要做有圖片、有表格的正式簡報，可以直接參考：

- `public/decks/riits/2026-06-04-image-table-template/slides.md`
- `public/decks/riits/2026-06-04-image-table-template/images/workflow.svg`

這份範本已示範：

- 如何把圖片放進 `images/`
- 如何在 `slides.md` 裡引用相對路徑圖片
- 如何用 HTML table 做比較完整的表格版面

## Markdown 與深層投影片

- 簡報內容一律寫在 `slides.md`
- 水平投影片使用 `---`
- 垂直投影片使用 `--`
- 模板已內建 reveal.js 的 Markdown 插件，適合做同類型簡報的延伸版本
- 表格優先使用 Markdown table；如果版面較複雜，也可以直接在 `slides.md` 放 HTML table

## 如果你要我幫你新增一份簡報

你只要提供以下素材，我就可以直接幫你整理成新的 deck：

- 主題名稱
- 日期
- 客戶名稱
- 每一頁的內容草稿
- 表格資料
- 圖片檔案
- 你想要的投影片順序

建議的資料夾內容如下：

```text
<topic-folder>/
  ├─ slides.md
  ├─ images/
  ├─ assets/
  └─ notes.md (可選，放備註或講稿)
```

## 密碼保護

- 每份 deck 都可以有自己的密碼
- 密碼資料集中在 `public/data/auth-config.json`
- 變更密碼時，只需要更新對應 key 的 SHA-256 雜湊與提示文字
- 模板頁目前是公開頁，不需要密碼

範例：

```bash
node -e "const c=require('crypto'); console.log(c.createHash('sha256').update('你的新密碼').digest('hex'));"
```

## 更新與下架

- 更新 deck：直接修改該 deck 的 `slides.md`
- 下架：將 `status` 改成 `archived`
- 草稿：將 `status` 改成 `draft`

## 注意事項

- 建議 slug 使用小寫英文與 `-`
- deck 路徑要與 `decks.json` 的 `path` 完全一致
- `npm run dev` / `npm run build` 都會先同步 reveal.js 靜態資產到 `public/vendor/reveal`
