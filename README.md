# Agentforce World Tour Tokyo 2026 Winter - Workshop Contents

当該リポジトリは下記イベントにおけるワークショップで利用する資材を格納したリポジトリである。

## イベント概要

- イベント : [Agentforce World Tour Tokyo - AIの確率論を決定論的実行力でビジネス成果に変える企業へ](https://www.salesforce.com/jp/events/world-tour/tokyo/)
- 日時 : 2026-11-18 ~ 2026-11-19
- 会場 : ザ・プリンス パークタワー東京 | Salesforce+

## ワークショップ概要

- ワークショップ : 2-WS-4 Agentforce VibesでReactの画面を作ってみよう！
- 概要 : 「Salesforce Multi-Framework」は、Salesforce上でReact等のモダンWebフレームワークを直接活用できる待望の機能です。本ワークショップではUI構築の実践にフォーカスし、この機能の登場によって可能になった「React画面の開発」を皆様に体験していただくことを目的としています。構築の手段としては「Agentforce Vibes」によるVibe Codingを採用し、Salesforceで動作するReact画面をみなさんと一緒に作り上げます。
- 日時 : 2026-11-19 14:30 ~ 15:30 JST

## ワークショップ手順

ワークショップの手順は下記の流れで行う。

### 全体の流れ

Agentforce Vibesに指示書（プロンプト）を渡し、TaskをStatus×Priorityの2軸で管理するKanban画面を、2つのPhaseに分けて作成する。

| ステップ | 内容 | 指示書 | 目安時間 |
| --- | --- | --- | --- |
| Phase-0 | 開発環境を準備する | - | 10分 |
| Phase-1 | モックデータでKanban画面を作成し、Salesforce組織へデプロイする | [prompts/prompt-1.md](prompts/prompt-1.md) | 20分 |
| Phase-2 | Kanban画面をSalesforceのTaskと接続し、Drag & Dropの結果を保存する | [prompts/prompt-2.md](prompts/prompt-2.md) | 15分 |

### Phase-0 : 開発環境を準備する

1. "[Quick Start: Troubleshoot Code with Agentforce Vibes - Create Your Development Environment](https://trailhead.salesforce.com/content/learn/projects/quick-start-troubleshoot-code-with-dev-agent/create-your-local-development-environment)"へアクセスする
2. "**Create Playground**"を押下する
3. "**Yes, Create Playground**"を押下する
4. 少し時間が経つと"**Agentforce Vibes IDE**"のPlaygroundが作成される
5. "**Launch**"を押下し、"**Agentforce Vibes IDE**"のPlaygroundを起動する
6. "**Setup Menu**"から"**Agentforce Vibes**"を起動する
7. "Agentforce Vibes Terms & Conditions"において"**Accept**"を押下する
8. （環境が準備されるまで待機）
9. ファイルの作成者の信頼関係の確認において"**Yes, I trust the authors**"を押下する
10. EXPLORERのルートディレクトリに`prompts`という名称のディレクトリを作成する

### Phase-1 : モックデータでKanban画面を作成する

1. `prompts`ディレクトリに`prompt-1.md`という名称のファイルを作成する
2. [指示書:Phase-1](prompts/prompt-1.md)の内容を`prompt-1.md`に転記する
3. "Agentforce Vibes"のチャット欄に`@prompt-1.md に従って作業を実施してください。`と入力する（`@`を入力するとファイル添付が可能）
4. 入力したチャットを送信する（コマンドの実行について確認された場合は"Run"を押下する）
5. デプロイ完了レポートが提出されたことを確認してSalesforce組織のアプリケーションランチャーを開く
6. アプリケーションランチャーから作成されたアプリケーションを開く（見当たらない場合はプロファイルからアプリケーションの割り当てを実施）
7. アプリケーションでDrag&Dropが機能することを確認する

### Phase-2 : SalesforceのTaskと接続する

1. `prompts`ディレクトリに`prompt-2.md`という名称のファイルを作成する
2. [指示書:Phase-2](prompts/prompt-2.md)の内容を`prompt-2.md`に転記する
3. "Agentforce Vibes"のチャット欄に`@prompt-2.md に従って作業を実施してください。`と入力する（`@`を入力するとファイル添付が可能）
4. 入力したチャットを送信する（コマンドの実行について確認された場合は"Run"を押下する）
5. デプロイ完了レポートが提出されたことを確認してSalesforce組織のアプリケーションランチャーを開く
6. アプリケーションランチャーからPhase-1で作成したアプリケーションを開く
7. Kanban画面にSalesforceのTaskが表示されることを確認する（Taskが0件の場合は、SalesforceでTaskを作成してから画面を再読み込みする）
8. TaskをDrag&Dropした後に画面を再読み込みして変更したStatusとPriorityが維持されていることを確認する
