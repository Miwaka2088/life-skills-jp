# store-hours-check(営業時間の裏取り)

> **English summary**
> A Claude Skill that makes Claude verify store and venue opening hours, regular closing
> days, and temporary closures from primary sources (the store's own page, official social
> accounts, Google Maps) instead of trusting roundup articles. Missing information is
> labeled "unknown", never assumed to mean "closed", and lists of candidate places separate
> verified from unverified entries. Written in Japanese.

「行ってみたら閉まっていた」を防ぐための Claude Skill です。
営業時間・定休日・臨時休業を、まとめ記事ではなく**公式サイトの店舗ページ・公式SNS・Googleマップ**で確認し、
どこで確認したか、確認できなかったことは何かを必ず添えて答えるようにします。

## こんなときに使う

- 「○○って今日やってる?」
- 「この店、何時まで?ラストオーダーは?」
- 「年末年始は営業してる?」
- 「近くで今から入れるカフェを3つ挙げて」
- 「祝日だけど、この施設は開いてる?」

## できること

- **現在の日付・曜日・時刻を確認してから答える**(祝日・年末年始などの特別営業にも注意)
- **チェーン店は店舗を特定してから調べる**
- **情報源を優先順に確認する**: 公式の店舗ページ → 公式SNSの直近の投稿 → Googleマップ → 予約サイト
- **食い違いがあれば伝える**(公式サイトと Google マップで時間が違う、など)
- **候補を挙げるときは、確認済みと未確認を分けて表にする**

## 特徴

- **まとめ記事を根拠にしない。** 店の候補を探すきっかけにだけ使う
- **「載っていない」は「休み」ではない。** 情報がない項目は「不明・未確認」と書く
- **確認した日付を書く。** 「11/3 時点の公式サイト」のように
- **確実に知りたいときは電話をすすめる**

## 導入

### いちばん簡単な方法

Claude に、このリポジトリの URL を渡してこう頼んでください。

> https://github.com/Miwaka2088/life-skills-jp の store-hours-check を導入して

手順は [SETUP_FOR_CLAUDE.md](../../SETUP_FOR_CLAUDE.md) に書いてあり、Claude がそれを読んで進めます。

### 手動で入れる

- **Claude.ai / Claude アプリ**: [`dist/store-hours-check.skill`](../../dist/store-hours-check.skill) をダウンロードし、設定の「スキル」からアップロード
- **Claude Code**:
  ```bash
  git clone https://github.com/Miwaka2088/life-skills-jp.git
  cp -r life-skills-jp/skills/store-hours-check ~/.claude/skills/
  ```

## 必要なもの

- Claude のウェブ検索が使えること

設定ファイルは不要です。入れればすぐ使えます。
