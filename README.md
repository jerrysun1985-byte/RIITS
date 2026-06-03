# RIITS 客戶簡報平台

這是一個專為客戶簡報設計的 GitHub Pages 站點。每份簡報都會保留成獨立 HTML deck，並用 Markdown 內容做成可複製的模板，方便後續持續新增、更新與下架。

- 首頁入口：`https://jerrysun1985-byte.github.io/RIITS/`
- 模板頁：`https://jerrysun1985-byte.github.io/RIITS/templates/deck/`
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
- `public/assets/deck-loader.js`: reveal.js 啟動腳本
- `public/assets/auth-gate.js`: 密碼保護流程
- `scripts/sync-reveal-assets.mjs`: 將 npm 的 reveal.js 靜態資產同步到 `public/vendor`

## 新增簡報的標準流程

1. 複製模板

```bash
cp -R public/templates/deck public/decks/<client>/<yyyy-mm-dd-topic-slug>
```

2. 編輯 `public/decks/<client>/<yyyy-mm-dd-topic-slug>/slides.md`

3. 更新 `public/data/decks.json`

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

4. 本機預覽與建置

```bash
npm run dev
npm run build
```

5. Push 到 `main`

GitHub Actions 會自動建置並發布到 Pages。

## Markdown 與深層投影片

- 簡報內容一律寫在 `slides.md`
- 水平投影片使用 `---`
- 垂直投影片使用 `--`
- 模板已內建 reveal.js 的 Markdown 插件，適合做同類型簡報的延伸版本

## 密碼保護

- 每份 deck 都可以有自己的密碼
- 密碼資料集中在 `public/data/auth-config.json`
- 變更密碼時，只需要更新對應 key 的 SHA-256 雜湊與提示文字

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
