# gitlab-command

## GitLab 工作流（GitLab Flow）

### 1. 建立 Feature Branch

從 `main`（或 `master`）分支切出一個獨立的分支來開發新功能：

```bash
git checkout -b feature/my-feature
```

---

### 2. 開發完成後，建立 Merge Request (MR)

- 填寫**標題**與**描述**（說明這個 MR 做了什麼）
- 指定 **Assignee**（通常是自己）和 **Reviewer**

#### Reviewer 人數建議

| 情境 | 建議 Reviewer 人數 |
|------|-------------------|
| 一般 MR | 至少 1～2 位 |
| 新人的 MR | 前幾次建議加上主管，作為 mentoring |

---

### 3. Review → Approve → Merge 流程

1. Reviewer 審查程式碼並按下 **Approve**
2. Assignee 確認後點擊 **Merge**
3. 是否勾選 **Delete source branch**，視情況決定

---

## Merge 策略比較

### 三種 Merge 模式說明

| 模式 | 說明 |
|------|------|
| **Merge Commit**（預設） | 保留分支上所有的 commits，並在 main 上額外產生一個 merge commit 來連結兩條線 |
| **Squash Merge** | 將分支上所有 commits 壓縮成一個新 commit 加到 main，原分支 commits 不會出現在 main 歷史中 |
| **Fast-forward** | 分岔後若 main 沒有新的提交，直接將 main 指標移動到分支最新節點，歷史保持線性 |

### 各模式對分支節點的影響

| 模式 | 原分支節點去向 |
|------|--------------|
| **Merge Commit** | 分支節點保留在原分支線，main 新增一個 merge commit |
| **Fast-forward** | 分支節點直接納入 main 的線性歷史，main 指標向前移動 |
| **Squash Merge** | 分支節點不進入 main 歷史，僅產生一個壓縮後的新 commit |

---

### ⚠️ Squash Merge 注意事項

Squash Merge 會將分支所有 commits 壓縮為一個全新的 commit，導致原本的 commit ID 與 main 斷開關聯。

若之後繼續在原有的 feature 分支開發，**會產生衝突**，建議 Merge 後刪除本地的 feature 分支：

```bash
git branch -d feature/my-feature
```

---

## git rebase 使用時機

當發現 `main` 已有他人新的提交時，可以透過 `rebase` 將自己的 commits 接到最新的 `main` 之後，保持線性歷史：

```bash
git checkout feature/111
git add .
git commit -m "Simple commit"
git pull --rebase origin main
git push --force-with-lease origin feature/111
```

---

## Merge 後清理舊分支

Merge 完成後建議刪除舊分支，避免分支過多造成混淆：

```bash
git branch -d feature/old-task
```

---

## 常用操作流程

### ▶ 新資料夾推上 GitHub

```bash
git init
git remote add origin <目標 Git URL>
git pull origin main
# 修改檔案後
git add .
git config user.name "使用者名稱"
git config user.email "電子信箱"
git commit -m "Initial commit"
git branch -m master main
git push -u origin main
```

---

### ▶ 在新資料夾切出獨立分支並推上 GitHub

```bash
git init
git remote add origin <目標 Git URL>
git pull origin main
git checkout -b feature/111
# 修改檔案後
git add .
git config user.name "使用者名稱"
git config user.email "電子信箱"
git commit -m "feat: 111"
git push -u origin feature/111
# feature 開發完成後，至 GitLab 建立 MR
```

---

### ▶ Merge 後接續開發（開始新的 Feature）

```bash
git checkout main
git pull origin main
git checkout -b feature/222
```
