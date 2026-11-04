# mono-in-out(買う前の判定と売り先選び)

> **English summary**
> A Claude Skill for owning fewer, better things. Before you buy, it checks the item against
> what you already own, your own buying criteria, cost per use, and resale value, then gives
> a one-line verdict (buy / skip / buy only if…) while leaving the decision to you. When you
> let something go, it picks the right channel for that kind of item (auction, flea-market
> app, specialist resale shop, large reuse chain), compares net proceeds and effort, and
> writes an honest listing. Prices and fees are always checked live. Written in Japanese.

**本当に必要なものだけを持って、身軽に暮らす**ための Claude Skill です。
買う前に一度立ち止まって判定し、手放すときは物の種類に合った売り先を選んで、手間なく・損なく手放します。

## こんなときに使う

- 「これ買うべき?迷ってる」
- 「同じようなの持ってるけど、新しいのが欲しい」
- 「これ売りたいんだけど、メルカリとヤフオクどっちがいい?」
- 「服をまとめて手放したい。どこに持っていけばいい?」
- 「出品文を書いて」

## できること

### 買う前の判定
- 何に困っているか・何の代わりかを聞く
- 手持ちとの重複を確認する
- 自分で決めた**買う基準**(家で洗える・使い切れる・売れる、など)に照らす
- **1回あたりの金額**と、売るときの見込みを引いた**実質の負担**を出す
- 見立てを1行で出す: 買う / 見送る / 条件付き(「○○を売ってから」など)

### 売り先選び
- 物の種類ごとに向いている売り先を選ぶ

  | 物の種類 | 向いている売り先の例 |
  |---|---|
  | 型番で探される物(PCパーツ・オーディオ・カメラ) | オークション |
  | 相場が安定した物(時計・スニーカー・ブランド小物) | フリマアプリ |
  | デザイナーズブランドの服 | そのブランドに強い専門の古着店 |
  | ノーブランドの物をまとめて | 大手リユース店 |

- 手数料・送料を引いた**手取り**と、**手間**で比べる
- 相場は**売り切れ価格**で見る

### 出品文
- 型番・サイズ・状態・付属品・使用回数を正直に書いたタイトルと説明文を作る

## 特徴

- **決めるのはユーザー。** 判定の材料と見立てを出すまで
- **「欲しい」を否定しない。** 基準に合うなら買ってよいと伝える
- **相場・手数料は毎回最新を確認する。** 記憶で答えない
- **出品・購入の操作はしない。** ボタンを押すのはユーザー
- **状態を良く書きすぎない。** 傷や使用感は正直に書く

## 導入

### いちばん簡単な方法

Claude に、このリポジトリの URL を渡してこう頼んでください。

> https://github.com/Miwaka2088/life-skills-jp の mono-in-out を導入して

手順は [SETUP_FOR_CLAUDE.md](../../SETUP_FOR_CLAUDE.md) に書いてあり、Claude がそれを読んで進めます。

### 手動で入れる

- **Claude.ai / Claude アプリ**: [`dist/mono-in-out.skill`](../../dist/mono-in-out.skill) をダウンロードし、設定の「スキル」からアップロード
- **Claude Code**:
  ```bash
  git clone https://github.com/Miwaka2088/life-skills-jp.git
  cp -r life-skills-jp/skills/mono-in-out ~/.claude/skills/
  ```

## 最初にやること

[`assets/mono-profile-template.md`](assets/mono-profile-template.md) に、自分の**買う基準**を書きます。
これが判定の軸になります。売った物・買った物は [`assets/mono-log-template.md`](assets/mono-log-template.md) の形で記録すると、
次に売るときの参考になります。

## 構成

```
mono-in-out/
├── SKILL.md                      # 本体
├── references/
│   ├── buy-check.md              # 買う前の判定
│   ├── sell-channels.md          # 売り先の選び方・値付け
│   └── listing.md                # 出品文の作り方
└── assets/
    ├── mono-profile-template.md  # 買う基準などの設定
    └── mono-log-template.md      # 物の出入りの記録
```
