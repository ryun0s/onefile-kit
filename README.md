# OneFile Kit

ビルド不要の単一HTMLアプリを作るための雛形と、その雛形から作った2つのアプリ。Kiroween 2026 の Skeleton Crew カテゴリ向けのキットです。

[紹介ページを開く](index.html)。ダウンロードしたフォルダでは、ルートの `index.html` をブラウザで開くと3つのファイルを選べます。各ファイルを個別に開いても使えます。スタイル、スクリプト、SVGはすべてHTMLに含まれています。

| ファイル | 使い方 |
| --- | --- |
| [Skeleton](skeleton/skeleton.html) | 新しいアプリの出発点。Settings で明暗テーマやアクセント色を試せます。 |
| [Tiny Habits](apps/tiny-habits/index.html) | New habit に40文字以内の名前を入力します。行の丸を押すと今日の達成を記録し、もう一度押すと取り消せます。ごみ箱は確認後に習慣とその履歴を削除します。 |
| [Focus Garden](apps/focus-garden/index.html) | 15・25・50分から選んで Start session を押します。Pause で一時停止すると、Resume で再開するか、End session で中止できます。 |

Tiny Habits の連続日数は、今日が未記録の間は昨日までの連続記録を表示します。下の表は今日を含む49日分で、ドットの濃さはその日に達成した習慣の数（0、1、2、3件以上）です。

Focus Garden では完了したセッションごとに芽が1つ増えます。その後さらに合計50分集中すると葉が茂り、100分で花が咲きます。進行中のタイマーはタブを閉じても終了予定時刻から復元されます。完了時刻が属する日の合計にセッションの分数が加わります。

## 雛形からアプリを作る

1. `skeleton/skeleton.html` をコピーします。例えば `apps/my-tool/index.html` として保存します。
2. `APP:CONFIG` の `START` と `END` の間で `APP.title`、説明、タブ名、アクセント色、初期データ `initial` を設定します。`key` は他のアプリと重複しない文字列にします。
3. `APP:BODY` に本文のHTMLを、`APP:CSS` に固有のスタイルを置きます。`--surface`、`--ink`、`--accent-ink` などのCSS変数を使うと設定が画面全体に反映されます。
4. `APP:SCRIPT` に操作と描画を書きます。`store.update` に渡す関数で `state.data` を編集すると、自動保存と購読者への通知が走ります。`store.subscribe` は登録時にも呼ばれるので、初期表示と再描画を同じ関数に書けます。

例えば `APP.initial` を `{ count: 0 }` にし、本文に `id="add"` のボタンと `id="count"` の出力を置くと、次の処理で回数を保存・表示できます。

```js
byId('add').addEventListener('click', () => {
  store.update(state => { state.data.count += 1; });
});
store.subscribe(({ data }) => {
  byId('count').textContent = String(data.count);
});
```

空状態には `emptyState({ title, message })` が返す要素を使えます。確認が必要な操作は `await confirmAction({ title, message, confirmLabel })` で選択を受け取り、完了後の短い通知は `toast(message)` で出します。保存形式を変更したときは `APP.restore(savedData)` で読み込み時に変換できます。

データと設定はアプリごとに分けて、開いたブラウザの localStorage に保存します。ローカルファイルでの保存の扱いはブラウザによって異なるので、続きから使うときは同じブラウザで同じ場所のHTMLを開いてください。フッターには開いたファイル名と配布時点のバイト数が表示されます。コピーを編集したら、`id="file-size"` の表示値を `00,000` に戻すと、次に開いたときに概算します。

## Kiroで仕様を使う

このフォルダをKiroのワークスペースとして開きます。[steering](.kiro/steering/) には目的、技術の制約、ファイル配置がまとまっています。[requirements.md](.kiro/specs/onefile-kit/requirements.md) で対象の要件IDを選び、[design.md](.kiro/specs/onefile-kit/design.md) と [DESIGN.md](DESIGN.md) で画面構成や色を確認してください。

例えば「H2に沿って、昨日まで3日連続で達成した習慣を今日チェックしたときの表示を確認して」と依頼すると、対象と期待する振る舞いを指定できます。変更の確認には [tasks.md](.kiro/specs/onefile-kit/tasks.md) を使い、該当するHTMLを開いて要件を満たした項目にチェックを付けます。
