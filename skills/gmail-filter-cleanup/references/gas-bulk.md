# Google Apps Script で過去メールを一括整理する

フィルタの「既存のスレッドにも適用」で足りない場合(数千件ある、条件が複雑など)に使う。
**一回限りの実行用。** 定期実行のトリガーは設定しない(振り分けはフィルタに任せる)。

## 準備(ユーザーが行う)

1. https://script.google.com で新しいプロジェクトを作る
2. 下のコードを貼り付ける
3. `RULES` を自分のルールに書き換える
4. 初回実行時に Gmail へのアクセス許可を求められるので、内容を確認して許可する

Claude は、ユーザーの承認なしに許可画面を操作しない。

## コード

```javascript
// 一括でラベルを付けて受信トレイから外す。メールは削除しない。
// DRY_RUN を true にすると、件数を表示するだけで何も変更しない。
const DRY_RUN = true;

const RULES = [
  // { query: Gmailの検索条件, label: 付けるラベル, archive: 受信トレイから外すか }
  { query: 'from:@shop.example', label: '買い物/通販', archive: true },
  { query: 'from:@card.example', label: 'お金/カード', archive: false },
];

// 検索の label: ではラベル名の「/」と空白が「-」になる
function searchLabel(name) {
  return name.replace(/[\/ ]/g, '-');
}

function bulkLabel() {
  const BATCH = 100;           // 1回に処理するスレッド数
  const MAX_PER_RULE = 2000;   // 1ルールあたりの上限(実行時間の制限対策)

  RULES.forEach(rule => {
    // ラベル済みのスレッドを除外して、同じスレッドを何度も処理しないようにする
    const query = `${rule.query} -label:${searchLabel(rule.label)}`;

    if (DRY_RUN) {
      const count = GmailApp.search(query, 0, 500).length;
      Logger.log(`[確認] ${query} → ${rule.label}: ${count}件${count === 500 ? '以上' : ''}`);
      return;
    }

    const label = GmailApp.getUserLabelByName(rule.label) || GmailApp.createLabel(rule.label);
    let done = 0;
    while (done < MAX_PER_RULE) {
      const threads = GmailApp.search(query, 0, BATCH);
      if (threads.length === 0) break;
      label.addToThreads(threads);
      if (rule.archive) GmailApp.moveThreadsToArchive(threads);
      done += threads.length;
    }
    Logger.log(`${rule.label}: ${done}件を処理${done >= MAX_PER_RULE ? '(上限に達したので再実行してください)' : ''}`);
  });
}
```

## 使い方

1. `DRY_RUN = true` のまま実行し、ログで件数を確認する
2. 件数がおかしくなければ `DRY_RUN = false` にして実行する
3. 実行時間の上限で止まった場合や、上限件数に達した場合は、もう一度実行すれば続きから処理される
4. 終わったら、このプロジェクトはそのまま残しておいてよい(次に一括整理が必要になったときに使う)

## 注意

- ラベル済みのスレッドは自動で除外されるので、何度実行しても同じスレッドを二重に処理しない
- 受信トレイのメールだけを対象にしたい場合は、条件に `in:inbox` を加える
- 削除の処理は書かない
