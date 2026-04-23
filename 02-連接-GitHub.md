---
title: 'Claude Code 懶人包 #02：連接 GitHub'
date: '2026-04-23'
type: 懶人包
version: v1.0
status: 正式版
tags:
  - Claude-Code
  - 懶人包
  - GitHub
  - GitHub-Pages
video: EP05
---
# Claude Code 懶人包 #02：連接 GitHub

> 版本：v1.0
> 更新日期：2026-04-23
> 對應影片：Claude基本功 EP05

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 GitHub，包括：

- 連接 GitHub 帳號，讓 Claude 可以直接操作
- 建立第一個教材 repo（網頁倉庫）
- 開啟 GitHub Pages，讓學生掃 QR Code 就能在手機上開啟
- 以後每次做好新教材，一句話就能上線

**完成後，你只要說：**
> 「幫我把這個教材推到 GitHub 上線，產生 QR Code」

Claude 就會自動搞定一切。

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 電腦有網路連線
- [ ] 有可用的 email 信箱（如果還沒 GitHub 帳號，懶人包會引導你註冊）

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code 桌面版的 Code 分頁，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。
>
> **所有安裝與設定都在 Claude Code 桌面版內完成，不需要另外打開 PowerShell 或命令提示字元。**

---

## 階段一：環境檢查

> 在開始所有操作之前，先確認環境正常。請逐一檢查，有問題就先修復，不要跳過。

### 步驟一：系統與工具確認

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行

2. **確認網路連線正常**：
   ```bash
   # Windows
   ping -n 1 github.com
   # macOS / Linux
   ping -c 1 github.com
   ```

3. **確認 Git 是否已安裝**：
   ```bash
   git --version
   ```
   未安裝時自動安裝：
   - Windows：`winget install --id Git.Git --accept-source-agreements --accept-package-agreements`
   - macOS：`xcode-select --install`
   - Linux：`sudo apt update && sudo apt install git -y`

4. **確認 GitHub CLI 是否已安裝**：
   ```bash
   gh --version
   ```
   未安裝時自動安裝：
   - Windows：`winget install --id GitHub.cli --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install gh`
   - Linux：`sudo apt install gh -y`

5. **確認 Git 使用者資訊是否已設定**：
   ```bash
   git config --global user.name
   git config --global user.email
   ```

> ✅ 全部通過後，告知使用者：「環境檢查完成，所有條件都符合，開始設定。」
>
> ⚠️ 若有工具剛安裝完找不到指令，提醒使用者**重啟 Claude Code 桌面版**後再繼續。

---

## 階段二：確認 GitHub 帳號

### 步驟二：詢問帳號狀態

> 🖐️ **需要手動操作**：請 Claude **先詢問使用者**：「你有 GitHub 帳號了嗎？」
>
> - **已有帳號** → 跳到階段三
> - **還沒有** → 依照以下流程引導使用者註冊（約 5 分鐘）

#### 沒有 GitHub 帳號的註冊引導

**Step 1：打開註冊頁面**

請使用者在瀏覽器開啟：https://github.com/signup

**Step 2：填寫資料**

| 欄位 | 說明 | 建議 |
|------|------|------|
| Email | 常用 email | 用學校或個人常用信箱 |
| Password | 至少 15 字元 或 8 字元含數字+小寫 | 請記下來或存到密碼管理器 |
| Username | 全站唯一，網址會用到 | **用英文+數字，不要中文**，例如 `wang-math`、`mathteacher2026` |
| Email preferences | 是否接收電子報 | 可以取消勾選（N） |

> ⚠️ **Username 一旦決定就很難改**（GitHub Pages 網址會跟著變），建議用「姓名+科目」這種容易記的英文。

**Step 3：通過驗證 + Email 確認**

- 完成拼圖驗證 → 點「Create account」
- 前往信箱 → 複製 8 位數驗證碼 → 貼回 GitHub 頁面
- 看到問卷選擇畫面，捲到最下方點 **Skip** 跳過
- 選擇 **Free 方案**（免費即可）

**Step 4：確認成功**

登入後看到 GitHub 首頁（Dashboard），即表示註冊完成。

> 🖐️ **常見問題**：
> | 問題 | 解法 |
> |------|------|
> | Username 被搶走了 | 加數字或連字號，例如 `wang-math-2026` |
> | 沒收到驗證信 | 檢查垃圾郵件匣，或點「Resend code」重寄 |
> | 拼圖驗證失敗 | 重新整理頁面再試 |

---

## 階段三：連接 Claude Code 與 GitHub

### 步驟三：登入 GitHub

先確認是否已登入：
```bash
gh auth status
```

若尚未登入，執行：
```bash
gh auth login --web --git-protocol https
```

> 🖐️ **需要手動操作**：
> 1. 終端機顯示一組驗證碼（例如 `ABCD-1234`），請記下
> 2. 瀏覽器應自動開啟 GitHub 授權頁面
> 3. 若未自動開啟，手動開啟 https://github.com/login/device
> 4. 輸入驗證碼 → 點「Authorize GitHub CLI」
> 5. 終端機顯示「✓ Logged in as [你的帳號]」即完成

確認登入狀態：
```bash
gh auth status
```

### 步驟四：設定 Git 使用者資訊

確認是否已設定：
```bash
git config --global user.name
git config --global user.email
```

若尚未設定：

> 🖐️ **需要手動操作**：請詢問使用者想顯示的姓名和 email。

```bash
git config --global user.name "你的姓名"
git config --global user.email "你的email"
```

---

## 階段四：建立第一個教材網頁並上線

### 步驟五：詢問教材資訊

> 🖐️ **需要手動操作**：請詢問使用者：
> 1. 這個教材的**主題**是什麼？（例如：二次方程式、光合作用、歐洲地理）
> 2. 這個 repo 想取什麼**名字**？（英文或數字，不含空格，例如 `math-quadratic`、`bio-photosynthesis`）

> 記下使用者的回答，後續步驟會用到。

### 步驟六：建立資料夾與教材網頁

在使用者的文件資料夾建立以下結構：

```
[repo名稱]/
├── index.html     ← 教材主頁
└── README.md      ← 說明文件
```

#### index.html 模板

根據使用者的教材主題，建立一個教師友善的教材網頁，使用以下模板（**請把[佔位符]替換為實際內容**）：

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[教材主題]</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: "Noto Sans TC", "Microsoft JhengHei", sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 24px 20px;
      line-height: 1.9;
      color: #333;
      background: #fafafa;
    }
    header {
      background: #2c5f8e;
      color: white;
      padding: 24px;
      border-radius: 8px;
      margin-bottom: 24px;
    }
    header h1 { font-size: 1.6rem; margin-bottom: 6px; }
    header p { opacity: 0.85; font-size: 0.9rem; }
    section {
      background: white;
      border-radius: 8px;
      padding: 20px 24px;
      margin-bottom: 20px;
      box-shadow: 0 1px 4px rgba(0,0,0,0.08);
    }
    h2 {
      color: #2c5f8e;
      font-size: 1.1rem;
      margin-bottom: 12px;
      padding-bottom: 6px;
      border-bottom: 1px solid #e0e0e0;
    }
    ul, ol { padding-left: 20px; }
    li { margin-bottom: 6px; }
    .note {
      background: #fff9e6;
      border-left: 4px solid #f5a623;
      padding: 12px 16px;
      border-radius: 4px;
      margin-top: 12px;
    }
    footer {
      text-align: center;
      color: #999;
      font-size: 0.8rem;
      margin-top: 32px;
    }
    @media (max-width: 480px) {
      body { padding: 16px 12px; }
      header h1 { font-size: 1.3rem; }
    }
  </style>
</head>
<body>

  <header>
    <h1>[教材主題]</h1>
    <p>[科目]｜[年級/班級]｜[日期]</p>
  </header>

  <section>
    <h2>學習目標</h2>
    <ol>
      <li>理解[目標一]</li>
      <li>能夠[目標二]</li>
      <li>應用[目標三]</li>
    </ol>
  </section>

  <section>
    <h2>教學內容</h2>
    <p>在這裡放你的主要教學內容...</p>
    <div class="note">
      💡 小提示：這個網頁可以在手機上正常瀏覽！
    </div>
  </section>

  <section>
    <h2>練習題</h2>
    <ol>
      <li>題目一</li>
      <li>題目二</li>
      <li>題目三</li>
    </ol>
  </section>

  <footer>
    <p>由 Claude Code 協助製作 ·
       <a href="https://github.com/[GitHub帳號]/[repo名稱]" style="color:#999">查看原始碼</a>
    </p>
  </footer>

</body>
</html>
```

#### README.md 模板

```markdown
# [教材主題]

[科目]教材，對應 GitHub Pages 網頁。

## 查看教材
GitHub Pages：https://[GitHub帳號].github.io/[repo名稱]/

## 更新方式
修改 index.html 後執行：
\`\`\`bash
git add index.html
git commit -m "更新教材內容"
git push
\`\`\`
```

> 請將所有 `[佔位符]` 替換為實際資訊，其中 `[GitHub帳號]` 先暫時填入，等步驟七取得後再確認。

### 步驟七：初始化 Git 並推送到 GitHub

```bash
cd [repo名稱]
git init
git add .
git commit -m "初版教材：[教材主題]"
gh repo create [repo名稱] --public --source=. --remote=origin --push
```

取得使用者的 GitHub 帳號名稱：
```bash
gh api user --jq '.login'
```

記下輸出的帳號名稱，後續產生網址和更新 README 會用到。

### 步驟八：開啟 GitHub Pages

執行以下指令開啟 GitHub Pages（使用 main 分支的根目錄）：

```bash
OWNER=$(gh api user --jq '.login')
REPO=[repo名稱]
gh api repos/$OWNER/$REPO/pages \
  --method POST \
  -H "Accept: application/vnd.github+json" \
  -f build_type=legacy \
  -f source[branch]=main \
  -f source[path]=/
```

> 若指令回傳錯誤，請改用手動方式：
> 🖐️ **手動開啟 Pages**：
> 1. 瀏覽器開啟 `https://github.com/[帳號]/[repo名稱]/settings/pages`
> 2. Source 選「Deploy from a branch」
> 3. Branch 選「main」，資料夾選「/ (root)」
> 4. 點「Save」

等待約 1～2 分鐘，查詢 Pages 網址：
```bash
gh api repos/$OWNER/$REPO/pages --jq '.html_url'
```

記下這個網址，例如：`https://wang-math.github.io/math-quadratic/`

> 🖐️ **需要手動操作**：請使用者在瀏覽器開啟這個網址，確認看到教材網頁。

---

## 階段五：產生 QR Code

### 步驟九：製作學生掃碼 QR Code

取得 GitHub Pages 網址後，產生 QR Code 讓學生直接掃碼開啟。

**方法一：用 Python 產生（推薦）**

先確認 Python 環境並安裝 qrcode 套件：
```bash
python --version 2>/dev/null || python3 --version
pip install qrcode[pil] 2>/dev/null || pip3 install qrcode[pil]
```

產生 QR Code 圖片：
```python
import qrcode

url = "https://[帳號].github.io/[repo名稱]/"
img = qrcode.make(url)
img.save("qrcode.png")
print(f"✅ QR Code 已儲存：qrcode.png")
print(f"網址：{url}")
```

> 執行完後告知使用者 qrcode.png 的儲存位置，可以投影或列印給學生掃描。

**方法二：線上產生（備用）**

若 Python 安裝有問題，告知使用者：
> 請前往 https://qr-code-generator.com 貼上你的 GitHub Pages 網址，下載 QR Code 圖片。

---

## 階段六：驗證與完成

### 步驟十：最終確認清單

請逐一確認以下項目：

1. ✅ Git 已安裝，版本正常
2. ✅ GitHub CLI 已安裝，`gh auth status` 顯示已登入
3. ✅ Git 使用者姓名與 email 已設定
4. ✅ 教材 repo 已成功推送到 GitHub
5. ✅ GitHub Pages 已開啟，可以用瀏覽器開啟教材網址
6. ✅ QR Code 已產生

全部通過後，告知使用者：

> 🎉 **完成！**
>
> 你的 Claude Code 已成功連接 GitHub。
>
> 你的教材網址是：`https://[帳號].github.io/[repo名稱]/`
>
> **以後每次更新教材只要說：**
> 「幫我把最新的教材推到 GitHub 上線，更新 QR Code」
> Claude 就會自動幫你更新網頁。

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + GitHub 會做的事 |
|----------|------------------------|
| 「幫我做一個互動遊戲並推到 GitHub 上線」 | 產生 HTML → 建 repo → 推送 → 開啟 Pages → 給你網址與 QR Code |
| 「幫我更新這個網頁的內容」 | 修改 index.html → commit → push → 自動更新 |
| 「幫我產生這個網頁的 QR Code」 | 產生 qrcode.png |
| 「幫我建一個新的教材 repo」 | 重複階段四流程，建立新 repo 並上線 |
| 「幫我列出我所有的 GitHub repo」 | `gh repo list` |

---

## 如果執行失敗，如何重來

對 Claude Code 說：
「GitHub 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 重新執行環境檢查
2. 確認 Git 和 GitHub CLI 是否正常
3. 確認 GitHub 登入狀態
4. 找出問題並修復

如果需要完全重置 GitHub CLI 登入：
```bash
gh auth logout
gh auth login --web --git-protocol https
```

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `gh: command not found` | 重啟 Claude Code 桌面版，或確認安裝路徑已加入 PATH |
| `git: command not found` | 重啟 Claude Code 桌面版，Windows 需確認 Git for Windows 已安裝 |
| `gh auth login` 瀏覽器沒開啟 | 手動開啟 https://github.com/login/device 並輸入驗證碼 |
| GitHub Pages 網頁顯示 404 | 等待 2～3 分鐘再重新整理，Pages 部署需要時間 |
| Pages API 回傳錯誤 | 改用手動方式在 GitHub 網頁上開啟（見步驟八） |
| push 被拒絕 | 確認 `gh auth status` 有顯示 `repo` 權限 |
| qrcode 安裝失敗 | 改用線上產生器（見步驟九方法二） |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、跨平台支援、測試 repo 驗證、GitHub 帳號註冊引導 |
| 2026-04-23 | v1.0 | 改為分階段結構、加入教材 HTML 模板（RWD）、修正 Pages API 指令、加入 QR Code 產生、加入完整驗證清單 |

---

## 相關連結

- [[00-環境建置|懶人包 #00：環境建置]]
- [[01-連接 NotebookLM|懶人包 #01：連接 NotebookLM]]
- [[Claude基本功EP05 - GitHub懶人包與教學網頁上線]]
- [[README|Claude Code 懶人包索引]]
