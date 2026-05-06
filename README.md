# 台美日港 跨市場族群連動清單 — 靜態網站

從台股投資人視角，看半導體與相關產業在美股、日股、港股有哪些對應標的。

## 檔案

- `index.html` — 主頁面（自包含, 含內嵌 JSON 資料）
- `data.json` — 純 JSON 資料（給其他應用使用）

## 本機預覽

```bash
cd web
python -m http.server 8000
# 瀏覽 http://localhost:8000
```

或直接雙擊 `index.html` 開啟（多數瀏覽器 OK，但部分功能需 HTTP server 才會 work）。

## 部署到 GitHub Pages

### 方案 A：獨立 repo (推薦)

```bash
# 建立新 repo
cd web
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<你>/<repo-name>.git
git push -u origin main
```

然後到 GitHub repo → Settings → Pages：
- Source: `main` branch / `/ (root)` folder
- 等 1-2 分鐘，部署到 `https://<你>.github.io/<repo-name>/`

### 方案 B：併到既有 repo 的 docs/ 子資料夾

```bash
# 從專案 root
mkdir -p docs
cp web/* docs/
git add docs/
git commit -m "Add web frontend"
git push
```

GitHub Settings → Pages → Source: `main` / `docs/`

### 方案 C：用 GitHub CLI 一鍵部署

```bash
cd web
gh repo create my-stock-list --public --source=. --push
gh api -X POST /repos/{owner}/{repo}/pages -f source.branch=main -f source.path=/
```

## 更新資料

當主 Excel (`v6.xlsx`) 有變動時，回到 root 跑：

```bash
python _build_web.py
```

會重新讀 Excel → 產生新的 `index.html` + `data.json`。

## 自訂

- 主色調: 編輯 `index.html` 中 `:root` 的 `--primary` 變數
- 市場顏色: 編輯 `--tw / --us / --jp / --hk` 變數
- 改變預設展開的大類: 在 `renderBigCat` 函式裡的 `idx === 0` 條件
