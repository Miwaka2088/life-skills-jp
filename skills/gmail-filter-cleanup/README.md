# gmail-filter-cleanup(Gmail をフィルタ中心で整理)

> **English summary**
> A Claude Skill that cleans up a Gmail inbox so it keeps itself organized without AI in
> the loop. It audits labels (merge duplicates, nest into two levels, remove empty ones),
> designs and creates Gmail filters, applies them to old mail (with an optional one-off
> Google Apps Script for large backlogs), and runs a weekly review that turns leftover
> inbox mail into new filter rules. It never deletes mail and asks before every change.
> Written in Japanese.

Gmail の受信トレイを、**Claude がいなくても自動で振り分けられる状態**にするための Claude Skill です。
毎回 AI がメールを読んで分類するのではなく、振り分けのルールを Gmail のフィルタとして登録し、
週に1回だけ「フィルタから漏れたメール」を見てルールを足していきます。

## こんなときに使う

- 「受信トレイがごちゃごちゃなので整理したい」
- 「ラベルが増えすぎたので作り直したい」
- 「通販やカードのメールを自動で振り分けたい」
- 「何千通もある過去のメールをまとめて整理したい」
- 「今週受信トレイに残ったメールを見て、フィルタを見直して」

## できること

- **ラベルの棚卸し**: 件数と最終受信日つきで一覧にし、統合・階層化・削除候補に分ける
- **ラベル構成の設計**: 「後で探すときの棚」として2段までの構成にする
- **フィルタの作成**: 条件を作る前に検索して、引っかかるメールを確認してから登録する
- **過去メールの一括整理**: 件数が多いときは、一回限りの Google Apps Script で処理する(最初は件数確認だけのモードで動く)
- **週1回の見直し**: 受信トレイに残ったメールを送信元ごとに集計し、新しいフィルタ・修正・配信停止の候補を出す
- **記録**: 作ったフィルタとその目的を残す

## 特徴

- **フィルタ中心。** 普段の振り分けは Gmail 自体がやるので、AI の利用料も手間もかからない
- **削除しない。** ラベル付けとアーカイブだけ。削除が必要ならユーザーが自分で行う
- **確認してから変える。** フィルタ作成・ラベル削除は、一覧を見せて承認を取ってから
- **重要なメールは受信トレイに残す。** 請求・本人確認・セキュリティ通知はラベルだけ付けてスキップしない
- **使っていないサービスはラベルを作らない。** 受信トレイに来れば、退会や解約を判断するきっかけになる

## 導入

### いちばん簡単な方法

Claude に、このリポジトリの URL を渡してこう頼んでください。

> https://github.com/Miwaka2088/life-skills-jp の gmail-filter-cleanup を導入して

手順は [SETUP_FOR_CLAUDE.md](../../SETUP_FOR_CLAUDE.md) に書いてあり、Claude がそれを読んで進めます。

### 手動で入れる

- **Claude.ai / Claude アプリ**: [`dist/gmail-filter-cleanup.skill`](../../dist/gmail-filter-cleanup.skill) をダウンロードし、設定の「スキル」からアップロード
- **Claude Code**:
  ```bash
  git clone https://github.com/Miwaka2088/life-skills-jp.git
  cp -r life-skills-jp/skills/gmail-filter-cleanup ~/.claude/skills/
  ```

## 必要なもの

- Claude から Gmail を読めること(Gmail のコネクタ、または Gmail にサインイン済みのブラウザ)
- 過去メールを大量に整理する場合のみ: Google Apps Script(script.google.com)。手順は [`references/gas-bulk.md`](references/gas-bulk.md)

## 構成

```
gmail-filter-cleanup/
├── SKILL.md                          # 本体(流れと守ること)
├── references/
│   ├── label-design.md               # ラベル構成の設計と見直し
│   ├── filters.md                    # フィルタの作り方、検索演算子
│   ├── gas-bulk.md                   # 過去メールの一括整理スクリプト
│   └── weekly-review.md              # 週1回の見直し
└── assets/
    └── filter-rules-template.md      # フィルタとルールの記録
```
