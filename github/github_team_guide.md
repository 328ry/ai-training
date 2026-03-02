# GitHub チーム開発ガイド（Opus.net）

このガイドを読めば、GitHub でのチーム共同作業ができるようになります。

---

## 1. Git と GitHub って何？

| 用語 | 一言で | たとえ |
|---|---|---|
| **Git** | ファイルの変更履歴を記録する仕組み | ゲームのセーブシステム |
| **GitHub** | その記録をチームで共有するクラウド | Google ドライブ的な場所 |
| **ローカル** | 自分のPC | 手元のノート |
| **リモート** | GitHub上（クラウド） | クラウドに保存したノート |
| **リポジトリ** | 1つのプロジェクト = 1つのフォルダ | 案件ごとのフォルダ |

---

## 2. 覚える操作は6つだけ

```bash
# ① 新しいブランチを作って移動（作業開始時）
git checkout -b feature/○○

# ② 変更したファイルを「次のセーブ」に含める
git add .

# ③ セーブする（メッセージをつけて）
git commit -m "やったことを書く"

# ④ GitHub にアップロード
git push -u origin feature/○○

# ⑤ main ブランチに戻る
git checkout main

# ⑥ GitHub から最新を取得
git pull
```

### 操作の流れ

```
  ファイルを編集
    ↓
  git add .             ← 「これをセーブに含めて」
    ↓
  git commit -m "..."   ← セーブ確定（PC内）
    ↓
  git push              ← GitHub にアップ
```

---

## 3. ブランチ = 下書きスペース

**main ブランチ**は本番です。直接触りません。

作業するときは**ブランチ（下書き）**を作って、そこで作業します。

```
main（本番） ──────────────────────────→
  ├── feature/top-page     ← 上田の作業場
  └── feature/contact-form ← Aさんの作業場
```

お互いの作業が干渉しないので安全です。

### ブランチの命名ルール

```
feature/○○○   → 新しい機能を作るとき
fix/○○○       → バグを直すとき
update/○○○    → 既存の機能を改善するとき
docs/○○○      → ドキュメントを変えるとき
```

---

## 4. チーム開発ワークフロー

**この流れに沿って作業してください。**

```
  ① Issue を確認する（何をやるか）
      ↓
  ② ブランチを作る
      git checkout main
      git pull
      git checkout -b feature/○○
      ↓
  ③ 作業する
      ファイルを編集 → git add . → git commit -m "..."
      （必要に応じて繰り返す）
      ↓
  ④ push する
      git push -u origin feature/○○
      ↓
  ⑤ GitHub で PR（Pull Request）を作る
      → GitHub のリポジトリページで「Compare & pull request」をクリック
      → タイトルと説明を書いて「Create pull request」
      ↓
  ⑥ レビューを受ける
      → チームメンバーが確認してコメント or 承認
      ↓
  ⑦ 承認されたらマージ
      → GitHub で「Merge pull request」をクリック
      ↓
  ⑧ ブランチを削除
      → GitHub で「Delete branch」をクリック
```

---

## 5. Pull Request（PR）の書き方

```
タイトル: ヘッダーのデザインを変更

説明:
## 変更内容
- ロゴの位置を左上に変更
- ナビメニューをハンバーガーメニューに変更

## 関連 Issue
Closes #3
```

- **タイトル**: 何をしたか一言で
- **説明**: なぜ・どう変えたか
- **Closes #n**: マージ時に Issue を自動で閉じる

---

## 6. トラブル対処法

### 「push できない！」

```
error: failed to push some refs
hint: Updates were rejected because the remote contains work that you do not have locally.
```

→ 誰かが先に push した。`git pull` してから再度 `git push`。

### 「コンフリクトが出た！」

ファイルにこんな表示が出たら：
```
<<<<<<< HEAD
自分の変更
=======
相手の変更
>>>>>>> feature/xxx
```

→ 残したい方を選んで、`<<<<<<<` `=======` `>>>>>>>` のマーカーを全部消す。
→ `git add .` → `git commit -m "コンフリクト解消"` → `git push`

### 「今どのブランチにいる？」

→ `git branch` で確認。`*` がついているのが今のブランチ。

### 「間違えて main で作業してしまった」（まだ push してない場合）

```bash
git stash              # 変更を一時退避
git checkout -b feature/○○  # ブランチ作成
git stash pop          # 退避した変更を戻す
git add . && git commit -m "..."  # 改めて commit
```

---

## 7. やってはいけないこと

- **main ブランチに直接 push しない**（必ずブランチ → PR → マージ）
- **他の人のブランチを勝手に変更しない**（自分のブランチだけ触る）
- **`git push --force` は使わない**（履歴が壊れる可能性がある）

---

## ルール まとめ

1. 作業は**必ずブランチ**で行う
2. main への反映は**必ず PR 経由**
3. PR には**わかりやすいタイトルと説明**を書く
4. 困ったら `git status` で状況を確認
5. わからないことは聞く（壊れることはないから安心して）
