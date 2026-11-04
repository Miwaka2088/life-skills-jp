# kakeibo-monthly-review(月次の家計レビュー)

> **English summary**
> A Claude Skill for a monthly household budget review in Japan. Once a month, collect
> credit card statements and bank transaction CSVs, decode cryptic bank memo fields
> (摘要), separate fixed and variable costs, compare against budget, track loan payoff
> progress, and pick one or two concrete changes for next month. It never guesses numbers,
> never logs in to banks for you, and never recommends financial products.
> Written in Japanese.

家計簿を毎日つけるのではなく、**月に1回、明細をまとめて振り返る**ための Claude Skill です。
カード明細・銀行の入出金CSV・ローン残高から、収支・カテゴリ別の支出・予算との差・ローン返済の進捗を1枚にまとめ、
来月変えることを1〜2個に絞って決めます。

## こんなときに使う

- 「今月のカード明細を貼るので家計レビューして」
- 「銀行のCSVを読んで、何にいくら使ったかまとめて」
- 「固定費を見直したい。サブスクと通信費を一覧にして」
- 「ローンの返済が予定どおり進んでるか見て」
- 「明細を集める日を決めたい。カードの締め日がバラバラ」

## できること

- **明細を集める日を決める**: カードごとの締め日・引落日から、全部の明細が確定する日を逆算する
- **銀行CSVの摘要を読み解く**: 決済代行会社の名前などで出てくる「よくわからない摘要」を、自分用の対応表で判定する
- **よくある取り違えを防ぐ**: カード引落とカード明細の二重計上、口座間の振替、利用月と引落月のずれ
- **固定費の見直し**: 月額換算の一覧、更新月、解約・乗り換え・レンタル買取の損益分岐を計算する
- **ローンの進捗**: 「月末残高が目標額以上減ったか」の1つの指標で見る。完済見込み・繰上返済の効果を計算する
- **毎月同じ形で出力**: 前月との比較がしやすい
- **記録**: 決めたことを日付つきで残し、翌月に効果を確認する

## 特徴

- **数字を推測しない。** 読めない金額・正体不明の支出は「データなし」「要確認」として聞く
- **責めない。** 予算超過は事実として書き、理由はユーザーに聞く
- **ログインしない。** 銀行・カード会社のサイトには入らず、明細はユーザーが用意する
- **金融商品を勧めない。** 計算して判断材料を示すまで。決めるのはユーザー

## 導入

### いちばん簡単な方法

Claude に、このリポジトリの URL を渡してこう頼んでください。

> https://github.com/Miwaka2088/life-skills-jp の kakeibo-monthly-review を導入して

手順は [SETUP_FOR_CLAUDE.md](../../SETUP_FOR_CLAUDE.md) に書いてあり、Claude がそれを読んで進めます。

### 手動で入れる

- **Claude.ai / Claude アプリ**: [`dist/kakeibo-monthly-review.skill`](../../dist/kakeibo-monthly-review.skill) をダウンロードし、設定の「スキル」からアップロード
- **Claude Code**:
  ```bash
  git clone https://github.com/Miwaka2088/life-skills-jp.git
  cp -r life-skills-jp/skills/kakeibo-monthly-review ~/.claude/skills/
  ```

## 最初にやること

[`assets/household-profile-template.md`](assets/household-profile-template.md) を自分用に埋めます。
わからない項目は空欄のままで大丈夫です。初回のレビューで Claude が聞きながら埋めます。

- 給料日、カードごとの締め日・引落日
- 銀行の摘要の対応表(初回は Claude が「これは何ですか」と聞いて作ります)
- 固定費、カテゴリ別の予算

Claude.ai ならプロジェクトのナレッジに、Claude Code なら作業フォルダに置いてください。

## 構成

```
kakeibo-monthly-review/
├── SKILL.md                          # 本体(流れと守ること)
├── references/
│   ├── statement-collection.md       # 明細を集める日の決め方、資料ごとの注意
│   ├── bank-csv.md                   # 銀行CSVと摘要の読み方
│   ├── review-output.md              # レビューの出力形式
│   ├── debt-progress.md              # ローン返済の進捗と計算
│   └── fixed-costs.md                # 固定費の見直し
└── assets/
    └── household-profile-template.md # 自分用に埋める設定
```

## 注意

- 計算と記録までを行う Skill です。ローン・投資・保険などの判断は、公式情報を確認のうえご自身で行ってください
- 明細には個人情報が含まれます。共有する範囲に気をつけてください
