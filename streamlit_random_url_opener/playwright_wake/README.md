# Playwright Wake Button Mode

Google Apps Script版は軽くて安定していますが、ブラウザではないため、Streamlit Cloudの sleep 画面に出る `Yes, get this app back up!` ボタンはクリックできません。

このフォルダは、GitHub Actions上でChromiumを起動し、各URLを開いて、sleep画面だった場合だけwakeボタンをクリックする強化版です。

## 動き

- Chromiumで各URLを開きます。
- ページ本文に `This app has gone to sleep due to inactivity` があるか確認します。
- sleep画面なら `Yes, get this app back up!` をクリックします。
- sleep画面でなければ、そのまま「起きている」と判定します。

## GitHub Actionsで使う

このディレクトリ全体をGitHubリポジトリに置き、`.github/workflows/wake-streamlit-apps.yml` もリポジトリ直下に置くと、4時間ごとに自動実行されます。

手動実行したい場合は、GitHubのリポジトリ画面で:

1. `Actions` を開く
2. `Wake Streamlit apps` を選ぶ
3. `Run workflow` を押す

## ローカルで試す

```bash
python -m pip install -r playwright_wake/requirements.txt
python -m playwright install chromium
python playwright_wake/wake_streamlit_apps.py --dry-run
python playwright_wake/wake_streamlit_apps.py --randomize-order --max-delay-seconds 20
```

## 注意

- GitHub Actionsのスケジュール実行は、指定時刻ぴったりではなく数分遅れることがあります。
- Chromiumを起動するため、Apps Script版より重いです。基本はApps Script版を常用し、このPlaywright版はsleep救援用に使うのが現実的です。
- URLを変更した場合は、`urls.json` も更新してください。
