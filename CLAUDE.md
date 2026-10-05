# PM Workspace Operational Guidelines

## 1. 目的
本リポジトリは、PM個人のタスク管理、複数プロジェクトの進捗追跡、
および定例報告資料の作成を行うためのコンテキスト統合空間である。

## 2. ディレクトリ構成
- /Daily Notes/YYYY-Wxx/YYYY-MM-DD.md : 週別の作業ログ・日次メモ（日常メモの起点）
- /Projects/[Project_Name]/ : 案件別のドキュメント（docs/）・議事録（meetings/）・障害記録（incidents/）
- /Tasks/todo.md : 全案件から抽出された統合タスク
- /Tasks/done/YYYY-MM.md : 完了タスクの月別アーカイブ
- /Context/ : 関係者（people.md）・用語（glossary.md）・文体ルール（writing-style.md）
- /Inbox/ : 未整理の取り込み情報（会議・チャットの要点、PDF化した資料）
- /.claude/skills/ : 定型業務のスキル（/add-new-daily-note 等で呼び出す）
- 各フォルダーの README.md に索引がある。まず README.md を読み、全ファイルの走査は避けること。

## 3. 作業ルール & 禁止事項
- 【必須】指示を受けた際、まずは /Daily Notes の最新ログを確認すること。
- 【必須】タスクの更新が発生した場合、/Tasks/todo.md を必ず同期すること。
- 【必須】案件の追加・終了・状態変化があった場合、/Projects/README.md の案件一覧を更新すること。
- 【必須】上司・社外向けの文面は /Context/people.md と /Context/writing-style.md に従うこと。
- 【必須】/Context/glossary.md にない略語が出てきたら、意味をユーザーに確認して追記を提案すること。
- 【必須】作業が終わったら、変更したファイルの一覧を報告すること（コミットはユーザーが確認してから）。
- 【禁止】ユーザーの明示的な許可なく /Projects 内のファイルを直接削除・上書きしてはならない。
- 【禁止】上司・社外向けの文面作成において、カジュアルなトーン（タメ口等）を使用してはならない。
- 【禁止】git push、ブランチ削除、履歴の書き換えをユーザーの許可なく行ってはならない。
