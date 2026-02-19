# Git Bash / Git コマンド一覧（チートシート）

>こちらは**gitbash**を想定して作成しています
> 
> gitコマンドはgitのbinなので共通だよ！！

---

## ① 基本操作（変更の流れ）

### 状態確認
git status

### 差分確認
git diff
git diff --staged

### 追加（ステージ）
git add .
git add ファイル名

### コミット
git commit -m "message"
git commit -m "docs: update markdown refs #15"

### 直前コミットのメッセージ修正（push前推奨）
git commit --amend -m "new message"

---

## ② ブランチ操作

### ブランチ一覧
git branch

### ブランチ作成して切り替え
git switch -c feature/xxx

### 切り替え
git switch main

### ブランチ削除（ローカル）
git branch -d ブランチ名        # マージ済みのみ
git branch -D ブランチ名        # 強制

---

## ③ リモート操作（GitHubと同期）

### リモート確認
git remote -v

### 取得（取り込まない）
git fetch

### 取得＋取り込み（マージ）
git pull

### 取得＋取り込み（rebase：履歴きれい）
git pull --rebase

### 送信
git push

### 初回 push（上流設定）
git push -u origin main

---

## ④ stash / rebase（作業退避・履歴整理）

### 作業を一時退避（未追跡も含む）
git stash -u

### 退避一覧
git stash list

### 戻す（適用してstashから消す）
git stash pop

### 戻す（適用するがstashは残す）
git stash apply

### rebase中に続行 / 中断
git rebase --continue
git rebase --abort

---

## ⑤ ログ確認（履歴を見る）

### ログ
git log

### 1行で簡潔に
git log --oneline

### グラフ表示（おすすめ）
git log --oneline --graph --decorate --all

### 特定ファイルの履歴
git log -- ファイル名

---

## ⑥ トラブル対処（よくあるやつ）

### add したのを戻す（ステージ解除）
git restore --staged .
git restore --staged ファイル名

### 変更自体を捨てる（作業ツリーを戻す）
git restore .
git restore ファイル名

### 追跡済みを無視対象にしたい（.gitignore追加後）
git rm -r --cached 対象フォルダ
git rm --cached 対象ファイル

### 直前コミットを取り消す（push前）
git reset --soft HEAD~1    # コミットだけ取り消し（変更は残す＆ステージ維持）
git reset HEAD~1           # コミット取り消し（変更は残す＆ステージ解除）

### 「git log」画面から抜ける
q

---

## Issueと連携（メッセージの型）

### 関連付け（自動クローズしない）
refs #15

### 自動クローズ（push後に閉じる）
closes #15
fixes #15

## 更新履歴
- 2026-2-18　新規作成    