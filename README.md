# life-skills-jp

> **English summary**
> A collection of Claude Skills for everyday life in Japan, written in Japanese:
> solo meal planning with weekly batch-prep, a monthly household budget review from card
> statements and bank CSVs, Gmail cleanup built around permanent filters, verifying store
> opening hours from primary sources, and deciding what to buy and where to sell what you
> no longer need. Personal settings live in fill-in templates, so each skill works for anyone.
>
> **To install**, give this repository's URL to Claude and ask it to install the skills.
> Claude follows [SETUP_FOR_CLAUDE.md](SETUP_FOR_CLAUDE.md).

日本の暮らしで使う Claude Skill 集です。
Claude と一緒に回してきた食事・メール・買い物のやり方をもとに、誰でも使える形に一般化しました。
家計の Skill は、一般的な月次レビューの手順としてまとめています。
個人に依存する設定は、すべてテンプレート(`assets/`)に分けてあります。

## Skill 一覧

| Skill | できること | こんなときに |
|---|---|---|
| [jisui-meal-planner](skills/jisui-meal-planner/) | 1週間の献立・買い物リスト・日曜の仕込み・毎晩のチェックリスト。在庫を週をまたいで引き継ぐ | 「今週の献立組んで」「豚こまが300g残ってる」 |
| [kakeibo-monthly-review](skills/kakeibo-monthly-review/) | カード明細・銀行CSVから月次の家計レビュー。摘要の読み解き、固定費の見直し、ローン返済の進捗 | 「今月の明細まとめて」「固定費を見直したい」 |
| [gmail-filter-cleanup](skills/gmail-filter-cleanup/) | ラベルの棚卸し、Gmailフィルタでの自動振り分け、過去メールの一括整理、週1回のルール見直し | 「受信トレイを整理したい」「フィルタ見直して」 |
| [store-hours-check](skills/store-hours-check/) | 営業時間・定休日・臨時休業を一次情報で確認。不明は不明と書き、確認済み/未確認を分ける | 「○○って今日やってる?」「年末年始は営業してる?」 |
| [mono-in-out](skills/mono-in-out/) | 買う前の判定(重複・使い切れるか・売れるか)と、物の種類に合う売り先選び・出品文 | 「これ買うべき?」「これどこで売るのがいい?」 |

## 共通の考え方

- **推測で埋めない。** 読めない数字・確認できない営業時間・わからない状態は「不明・未確認」と書く
- **現在日時を最初に確認する。** 献立・締め日・営業時間はすべて日付で決まる
- **決めるのはユーザー。** Claude は材料を並べて見立てを出すまで。購入・出品・送信・削除はユーザーが行う
- **続く仕組みにする。** 毎回 Claude が頑張るのではなく、ルール・テンプレート・記録を育てて回るようにする
- **淡々と事実で。** 責めない、過度に励まさない

## 導入

### いちばん簡単な方法: Claude に頼む

Claude(Claude.ai・Claude アプリ・Claude Code のどれでも)に、このページの URL を渡してこう頼んでください。

> https://github.com/Miwaka2088/life-skills-jp を導入して

1つだけ入れたいときは、名前を添えます。

> https://github.com/Miwaka2088/life-skills-jp の jisui-meal-planner を導入して

Claude が [SETUP_FOR_CLAUDE.md](SETUP_FOR_CLAUDE.md) を読んで、環境に合わせた導入と設定ファイルの準備を進めます。

### Claude.ai / Claude アプリで手動で入れる

1. [`dist/`](dist/) から使いたい Skill の `.skill` ファイルをダウンロードする
2. 設定の「スキル」からアップロードする
3. プロジェクトを作り、各 Skill の `assets/` にあるテンプレートを自分用に埋めてナレッジに入れる
4. 普段どおりに話しかける(Skill は必要なときに自動で使われます)

### Claude Code で手動で入れる

```bash
git clone https://github.com/Miwaka2088/life-skills-jp.git
cp -r life-skills-jp/skills/* ~/.claude/skills/
```

使いたいものだけコピーしても構いません。

## 構成

```
skills/
├── jisui-meal-planner/       # 自炊の献立・仕込み
├── kakeibo-monthly-review/   # 月次の家計レビュー
├── gmail-filter-cleanup/     # Gmailの整理
├── store-hours-check/        # 営業時間の裏取り
└── mono-in-out/              # 買う前の判定・売り先選び
dist/                         # Claude.ai にアップロードできる .skill ファイル
SETUP_FOR_CLAUDE.md           # Claude が読む導入手順
```

各 Skill は `README.md`(説明)、`SKILL.md`(本体)、`references/`(必要なときに読む詳細)、`assets/`(自分用に埋めるテンプレート)でできています。

## 注意

- 家計の Skill は計算と記録までを行い、金融商品を勧めることはしません。ローン・投資などの判断はご自身で、公式情報を確認のうえ行ってください
- 手数料・相場・営業時間などは変わります。Skill は最新の情報を確認するよう指示していますが、最終確認はご自身でお願いします
- 食物アレルギーや医療的な食事制限がある場合は、そちらを優先してください

## ライセンス

MIT — [LICENSE](LICENSE) を参照してください。
