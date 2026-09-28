# 指示書: Phase-1

Salesforce Multi-FrameworkのReactアプリとして、Taskを管理する2次元のKanban画面を作成してください。

この作業では、Reactアプリを実装するだけで終了せず、production buildを実行し、現在接続しているSalesforce組織へデプロイして、SalesforceのApp Launcherから確認できる状態まで完成させてください。

## 画面の目的

TaskをStatusとPriorityの2つの軸で俯瞰し、Drag & Dropで直感的に整理できる画面を作ります。

## UI要件

1画面のシンプルなKanban Boardにしてください。

横軸をTaskのStatusにしてください。

- Not Started
- In Progress
- Completed
- Waiting on someone else
- Deferred

縦軸をTaskのPriorityにしてください。

- High
- Normal
- Low

各Taskをカードとして、StatusとPriorityに対応するセルへ表示してください。

Taskカードには以下を表示してください。

- Subject
- ActivityDate

5列×3行のKanbanとして、StatusとPriorityの関係が一目で分かるデザインにしてください。

横幅が不足する場合でもレイアウトが崩れないようにしてください。

## Drag & Drop

Drag & Dropには `@dnd-kit/react` を使用してください。

必要な依存関係がまだインストールされていない場合は、以下に相当する処理を実行してください。

`npm install @dnd-kit/react`

旧APIである以下のパッケージは使用しないでください。

- `@dnd-kit/core`
- `@dnd-kit/sortable`
- `@dnd-kit/utilities`

`@dnd-kit/react` の現在のAPIを使用してください。

Taskカードをdraggableにし、Status × Priorityの各セルをdroppableにしてください。

Taskカードを別のセルへDropすると、移動先のセルに応じてTaskのStatusとPriorityが変更されるようにしてください。

例えば、

- 横方向への移動ではStatusが変わる
- 縦方向への移動ではPriorityが変わる
- 斜め方向への移動ではStatusとPriorityの両方が変わる

ようにしてください。

同一セル内でのTaskの並び替え機能は必要ありません。

## モックデータ

この段階ではSalesforceのTaskデータには接続しません。

Reactアプリ内のモックデータを使用してください。

5種類のStatusと3種類のPriorityにTaskが適度に分散するように、Drag & Dropの動作確認に適したモックTaskを用意してください。

Taskのモックデータには少なくとも以下を含めてください。

- Id
- Subject
- Status
- Priority
- ActivityDate

Salesforce APIやData SDKによるTaskの取得・更新は、この段階では実装しないでください。

## 実装上の条件

- ReactとTypeScriptを使用してください。
- Salesforce Multi-Frameworkのプロジェクト構成を維持してください。
- Drag & Dropには `@dnd-kit/react` を使用してください。
- Drag & Drop以外の目的で不要なnpmパッケージを追加しないでください。
- 不要な画面、ナビゲーション、チャートなどは追加しないでください。
- 過度に複雑な設計にはしないでください。
- ワークショップ参加者が生成後のコードを追える程度のシンプルな構成にしてください。

## production build

実装が完了したら、production buildを実際に実行してください。

UIBundleのディレクトリと `package.json` を確認し、このプロジェクトで定義されているbuild scriptを使用してください。

通常は以下に相当する処理です。

`npm run build`

TypeScriptエラーやbuildエラーが発生した場合は、そのままデプロイへ進まないでください。

エラーの原因を確認して修正し、production buildが正常に成功する状態にしてください。

UIBundleのデプロイ対象となるbuild成果物が正常に生成されたことを確認してください。

## Salesforceへのデプロイ

production buildが成功したら、現在Agentforce Vibesで認証済みかつデフォルトとして設定されているSalesforce組織へデプロイしてください。

別のSalesforce組織へ接続先を変更しないでください。

UIBundleだけではなく、ReactアプリをSalesforceのApp Launcherから利用するために必要なCustomApplicationやPermission Setなど、このReact Internal Appに必要な関連メタデータも含めてデプロイしてください。

Salesforceプロジェクトの構造を確認し、現在のプロジェクトに適したSalesforce CLIのデプロイ処理を実際に実行してください。

単にデプロイ方法やコマンドを説明するだけではなく、Agentforce Vibes自身がデプロイを実行してください。

デプロイに失敗した場合はエラー内容を確認してください。

今回作成したReactアプリ、UIBundle、CustomApplication、build成果物などに起因する問題であれば修正し、再度production buildとデプロイを実行してください。

Salesforce組織の設定や権限など、このプロジェクトのコードから安全に修正できない問題の場合は、組織設定を勝手に変更せず、必要な対応を明確に説明してください。

## アプリへのアクセス

デプロイが成功したら、デプロイ結果が成功していることを確認してください。

このReact Internal App用のPermission Setがプロジェクトに用意されている場合は、現在のユーザーがアプリを利用できるよう、必要に応じてそのPermission Setを現在のユーザーへ割り当ててください。

そのうえで、以下を確認してください。

- UIBundleが正常にデプロイされた
- CustomApplicationが正常にデプロイされた
- 現在のユーザーがアプリへアクセスできる
- ReactアプリがSalesforceのApp Launcherから利用できる状態になっている

## 最後の報告

すべての作業が完了したら、長い説明はせず、以下だけを簡潔に報告してください。

1. 作成したReactアプリの概要
2. 使用したDrag & Dropライブラリ
3. production buildの結果
4. Salesforceへのデプロイ結果
5. デプロイしたアプリ名
6. App Launcherからアプリを開くために参加者が行う操作

デプロイが成功していない場合は、成功したかのように報告せず、失敗した処理と原因を明確に示してください。
