# 定期実行の設定（業務用PCだけで登録する）

秘書の定期実行は **業務用PCだけ** に登録する。自宅PCには登録しない（2台で同時に動くと、同じファイルを別々に書き換えてしまい、git で衝突するため）。

## 登録のしかた
業務用PCの Claude Code デスクトップアプリでこのリポジトリを開き、「docs/scheduled-tasks.md の定期実行を登録して」と頼む。
AI は下の表の4つを、`<REPO>` をこのPCでのリポジトリの場所（絶対パス）に置き換えて登録する。
登録したら、サイドバーの「Scheduled」から各タスクの「今すぐ実行」を1回ずつ押し、Notion などのツールの許可を済ませておく（以後は許可待ちで止まらない）。

- アプリが起動している間だけ動く。閉じていた間の分は、次にアプリを起動したときに1回だけ実行される
- 開始時刻はアプリが数分ずらす（例：8:30 → 8:37）
- 完了通知はこの設定を頼んだ会話には送らない（notifyOnCompletion: false）。報告はサイドバーに出るセッションと PushNotification で受け取る

| taskId | 名前 | cron（平日・ローカル時刻） |
|---|---|---|
| secretary-notion-sync | 【秘書】Notion 週次ページの読み取り | `15,45 8-19 * * 1-5` |
| secretary-start-day | 【秘書】朝の業務開始 | `30 8 * * 1-5` |
| secretary-midday-check | 【秘書】昼の確認 | `0 13 * * 1-5` |
| secretary-end-day | 【秘書】一日の締め | `30 17 * * 1-5` |

## 各タスクの指示文
共通の前置き（4つとも先頭に付ける）：
```
あなたは PM の秘書として、作業フォルダー <REPO>（git リポジトリ）で動く。
1. まず <REPO>\CLAUDE.md を読み、そのルールに従う。
```

### secretary-notion-sync
```
2. <REPO>\.claude\skills\notion-sync\SKILL.md の手順をそのまま実行する（Notion コネクターの notion-search と notion-fetch を使う）。
守ること:
- Notion は読むだけ。絶対に編集しない。
- これは定期実行なので、ユーザーに質問しない。判断が必要なものは Tasks/notion-sync.md の「確認待ち」に入れるだけにする。
- 差分が無ければ Tasks/notion-sync.md の「最終同期」だけ更新して、すぐ終える。
- 電話番号・URL・添付は取り込まない。git commit・push はしない。
- 最後に「差分なし」または差分の要約と変更したファイルの一覧を3行以内で出力する。
```

### secretary-start-day
```
2. <REPO>\.claude\skills\start-day\SKILL.md の手順を実行する（最初に notion-sync の手順で Notion の今週の週次ページを読み取る。Notion は読むだけ）。
3. 手順4の提示（確認日が来た依頼、期限切れ、P? の優先度案、確認待ち、今日のトップ3の案）まで進めたら、ユーザーの返答を待つ。返答が来たら残りの手順を進める。
4. 提示の準備ができたら、PushNotification ツールが使えれば「朝の報告：確認日が来た依頼 N件・P? N件・確認待ち N件。トップ3を決めてください」と一言通知する。
報告は日本語で、結論から短く。優先度・完了扱い・依頼のクローズはユーザーの確認なしに変えない。メッセージの送信、git push はしない。
```

### secretary-midday-check
```
2. <REPO>\.claude\skills\midday-check\SKILL.md の手順を実行する（最初に notion-sync の手順で Notion の今週の週次ページを読み取る。Notion は読むだけ）。
3. 報告を出したらユーザーの返答を待ち、返答が来たら反映する。
4. 報告の準備ができたら、PushNotification ツールが使えれば「昼の確認：トップ3 残りN件・確認日が来た依頼 N件・確認待ち N件」と一言通知する。
確認メッセージは Teams・Outlook に貼り付けて使う下書きとして出す（Outlook・Teams には接続しない。送信はユーザー）。優先度・完了扱い・依頼のクローズはユーザーの確認なしに変えない。git commit・push はしない。
```

### secretary-end-day
```
2. <REPO>\.claude\skills\end-day\SKILL.md の手順を実行する（最初に notion-sync の手順で Notion の今週の週次ページを読み取る。Notion は読むだけ）。
3. 確認事項はまとめて1回で質問し、ユーザーの返答を待ってから反映する。
4. 報告の準備ができたら、PushNotification ツールが使えれば「一日の締め：確認事項 N件。明日の準備ができています」と一言通知する。
優先度・完了扱い・依頼のクローズはユーザーの確認なしに変えない。コミットと push はユーザーの許可が出てから行う。
```

## 自宅PCで作業するとき
- 作業を始める前に `git pull` で業務用PCの変更を取り込む（/start-day の最初にも AI が確認する）
- 定期実行は自宅PCでは動かない。必要なら `/start-day`・`/notion-sync` などを手動で実行する
- 作業を終えたら、コミットして push する（許可を出す）。翌朝、業務用PCの /start-day が pull して取り込む
