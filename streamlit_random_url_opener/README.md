# Streamlit URL Random Opener

21個のStreamlit URLを、PCに頼らずGoogle Apps Script上でランダムな時刻にアクセスする仕組みです。

## 何をするか

- 4時間ごとに新しいランダム予定表を作ります。
- 各URLに、その4時間内のランダムなアクセス予定時刻を1つずつ割り当てます。
- Google Apps Scriptの時間トリガーが15分ごとに動き、予定時刻を過ぎたURLだけをHTTP GETで開きます。
- 失敗時は同じ4時間枠内で最大3回まで再試行します。
- PCを起動し続ける必要はありません。

## セットアップ

1. <https://script.google.com/> を開き、新しいプロジェクトを作ります。
2. 初期状態の `Code.gs` を削除し、[google_apps_script/Code.gs](google_apps_script/Code.gs) の内容を貼り付けます。
3. 保存します。
4. 関数一覧から `install` を選び、実行します。
5. Googleの権限確認が出たら許可します。
6. 関数一覧から `showPlan` を実行すると、次の4時間枠のランダム予定表をログで確認できます。

これで完了です。以後はGoogle側のトリガーで自動実行されます。

## よく使う関数

- `install()`: 15分ごとの自動トリガーを作り、予定表を初期化します。
- `showPlan()`: 現在の4時間枠の予定表をログに表示します。
- `runNow()`: 手動テストとして、全URLへ今すぐアクセスします。
- `resetSchedule()`: 現在の4時間枠の予定表を作り直します。
- `uninstall()`: 自動トリガーと保存済み予定表を削除して停止します。

## 運用メモ

- これはブラウザ画面を実際に開くのではなく、GoogleのサーバーからHTTP GETでURLへアクセスします。Streamlitアプリを起こす用途なら通常はこちらで十分です。
- Google Apps Scriptの時間トリガーは秒単位で正確ではありません。実運用上は「ランダムな15分枠で開く」程度の精度です。
- URLを追加・削除したら `URLS` を編集し、`resetSchedule()` を実行してください。
- 自分が管理しているアプリ、またはアクセス許可のあるURLだけに使ってください。ホスティング側の利用規約と負荷にも注意してください。

## sleep画面のボタンまで押したい場合

Apps Scriptはブラウザではないため、Streamlit Cloudの sleep 画面に出る `Yes, get this app back up!` ボタンはクリックできません。

その用途には [playwright_wake](playwright_wake) を使います。GitHub Actions上でChromiumを起動し、sleep画面だった場合だけwakeボタンをクリックします。
