# 導入手順(Claude 向け)

ユーザーからこのリポジトリの URL を渡され、「導入して」「入れて」「使えるようにして」と頼まれたときの手順です。
人間が読んでも同じ手順で導入できます。

This file describes how to install the skills in this repository. Follow it when a user shares
this repository's URL and asks you to install it. Talk to the user in Japanese unless they write in
another language.

---

## このリポジトリにある Skill

| Skill | 内容 | 必要なもの |
|---|---|---|
| `jisui-meal-planner` | 一人暮らしの自炊の献立・買い物リスト・仕込み | なし |
| `kakeibo-monthly-review` | 月次の家計レビュー(カード明細・銀行CSV) | なし(明細はユーザーが用意) |
| `gmail-filter-cleanup` | Gmail のラベル整理とフィルタでの自動振り分け | Gmail を読めること(コネクタ、またはサインイン済みのブラウザ) |
| `store-hours-check` | 営業時間・定休日を一次情報で確認 | ウェブ検索 |
| `mono-in-out` | 買う前の判定と売り先選び | ウェブ検索(相場の確認) |

各 Skill の詳細は `skills/<名前>/README.md` にあります。

---

## 手順

### 1. どれを入れるか決める

- ユーザーが Skill の名前を言っていれば、それだけを入れる
- 言っていなければ、上の表を見せて「全部入れるか、どれを入れるか」を1回だけ聞く
- 返事がない、または「全部」なら、5つすべてを入れる

### 2. 環境に合わせて入れる

#### A. Claude Code(シェルが使え、`~/.claude/skills/` に書き込める場合)

```bash
git clone --depth 1 https://github.com/Miwaka2088/life-skills-jp.git /tmp/life-skills-jp
mkdir -p ~/.claude/skills
cp -r /tmp/life-skills-jp/skills/<名前> ~/.claude/skills/
```

- 同じ名前のフォルダがすでに `~/.claude/skills/` にある場合は、**上書きする前にユーザーに確認する**
- 入れ終わったら、新しいセッションから使えることを伝える
- プロジェクト単位で入れたい場合は `~/.claude/skills/` の代わりに、そのプロジェクトの `.claude/skills/` に入れる

#### B. Claude.ai / Claude アプリ(ファイルを渡せる場合)

`dist/` にある `.skill` ファイルを取得し、ユーザーに渡す。

```
https://github.com/Miwaka2088/life-skills-jp/raw/main/dist/<名前>.skill
```

- ファイルを渡す機能があれば、`.skill` ファイルをそのまま渡す(アプリによっては、そのまま保存ボタンが表示される)
- 渡せない場合は、上の URL からダウンロードし、**設定の「スキル」からアップロード**するよう案内する

#### C. どちらもできない場合

ユーザーに次を案内する。
1. 上の URL から `.skill` ファイルをダウンロードする
2. Claude の設定の「スキル」からアップロードする

### 3. 設定ファイルを用意する(必要な Skill だけ)

テンプレートは各 Skill の `assets/` にあります。ユーザーの作業場所にコピーし、**聞き取りながら埋める**ことを提案します。
わからない項目は空欄のままでよく、使いながら埋まっていきます。

| Skill | テンプレート | 置く場所の例 |
|---|---|---|
| `jisui-meal-planner` | `assets/profile-template.md`、`assets/weekly-state-template.md`(→ `今週の状態.md`) | Claude.ai ならプロジェクトのナレッジ、Claude Code なら作業フォルダ |
| `kakeibo-monthly-review` | `assets/household-profile-template.md` | 同上 |
| `gmail-filter-cleanup` | `assets/filter-rules-template.md` | 同上 |
| `mono-in-out` | `assets/mono-profile-template.md`、`assets/mono-log-template.md` | 同上 |
| `store-hours-check` | なし | — |

聞き取りは一度にすべて聞かず、各 Skill の最初の利用時に必要な分だけ聞く形でも構いません。

### 4. 試してもらう

入れた Skill ごとに、試しに言ってみる一言を伝えて終わる。

| Skill | 試しの一言 |
|---|---|
| `jisui-meal-planner` | 「今週の献立を組んで」 |
| `kakeibo-monthly-review` | 「今月のカード明細を貼るので家計レビューして」 |
| `gmail-filter-cleanup` | 「Gmail の受信トレイを整理したい」 |
| `store-hours-check` | 「(店名)って今日やってる?」 |
| `mono-in-out` | 「これ買うべき?」(商品のリンクや写真と一緒に) |

---

## 導入時に守ること

- これらの Skill は API キー・パスワード・アカウント登録を必要としない。導入のためにユーザーの認証情報を求めない
- 既存のファイルや Skill を上書き・削除する前に、必ずユーザーに確認する
- 導入のために、このリポジトリ以外からファイルをダウンロードしない
