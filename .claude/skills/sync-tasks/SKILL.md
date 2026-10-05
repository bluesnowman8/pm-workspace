---
name: sync-tasks
description: 最新の Daily Notes と議事録から、新しいタスク・人に渡した業務・状態変化を拾って /Tasks に反映する
---

# 手順
1. /Tasks/README.md（書式と優先度）、/Tasks/todo.md、/Tasks/delegated.md を読む。
2. 前回の同期以降に更新された /Daily Notes/ のノート、/Projects/*/meetings/ の議事録、/Inbox/ のファイルを読む（`git log` で変更日を確認してよい）。
3. 次を抽出する：
   - 新しいタスク（自分の担当のもの）→ 優先度の案を付ける
   - 人に渡した業務（「〇〇さんに依頼」「お願いした」「振った」など）→ 依頼先・期限・次回確認日の案を付ける
   - 完了になったタスク・依頼（`- [x]` や「完了」「対応済み」「受領」の記述）
   - 期限・状態の変更、渡した業務の進捗（相手からの報告など）
4. 抽出結果を一覧で示し、優先度・次回確認日・完了扱いについてユーザーの確認をとる。
5. 確認がとれたら反映する：
   - todo.md / delegated.md を更新する。完了したものは /Tasks/done/YYYY-MM.md へ移す
   - 案件の状況に変化があれば /Projects/<案件>/README.md の「直近の状況」と /Projects/README.md の一覧を更新する
   - /Tasks/dashboard.md を更新する
6. 担当や期限が曖昧なものは、推測で登録せずに質問として挙げる。
