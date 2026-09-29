# 指示書: Phase-2

Prompt 1で作成し、Salesforce組織へデプロイ済みのReact製2次元Kanbanアプリを、Salesforceの実際のTaskデータと接続してください。

この作業では新しいReactアプリを作成せず、Prompt 1で作成した既存のアプリを更新してください。

現在のSalesforce Multi-Frameworkプロジェクト、UIBundle、CustomApplication、Permission Set、アプリ名、画面デザイン、Drag & Dropの操作性を維持したまま、モックデータをSalesforce Taskとのデータ連携に置き換えてください。

実装だけで終了せず、production buildを実行し、Prompt 1と同じSalesforce組織へ再デプロイして、SalesforceのApp Launcherから利用できる状態まで完成させてください。

## Salesforce Taskとの接続

Prompt 1で使用しているモックTaskを、Salesforceから取得したTaskに置き換えてください。

Salesforceとのデータアクセスには、Salesforce Multi-FrameworkのData SDKを使用してください。

現在のGA版である `@salesforce/platform-sdk` を使用し、Data SDKの機能は以下からimportしてください。

`@salesforce/platform-sdk/data`

古い `@salesforce/sdk-data` は使用しないでください。

プロジェクトに `@salesforce/platform-sdk` が既に含まれている場合は、その既存バージョンを使用してください。

Salesforce APIを通常の `fetch` や `axios` から直接呼び出さないでください。

GraphQLによるデータ取得には、Data SDKの現在のAPIである `dataSdk.graphql?.query()` を使用してください。

GraphQLによるデータ更新には `dataSdk.graphql?.mutate()` を使用してください。

必要に応じて `createDataSDK` や `gql` を使用してください。

## Salesforceからのデータ取得

SalesforceのTaskから少なくとも以下の項目を取得してください。

- Id
- Subject
- Status
- Priority
- ActivityDate

TaskのStatusは以下の5種類をKanbanの横軸として扱ってください。

- Not Started
- In Progress
- Completed
- Waiting on someone else
- Deferred

Priorityは以下の3種類を縦軸として扱ってください。

- High
- Normal
- Low

取得件数が過度に多くならないようにし、最大100件程度を目安にしてください。

可能であれば最近更新されたTaskが優先されるようにしてください。

取得したTaskを、StatusとPriorityに対応したPrompt 1の5列×3行のKanbanセルへ表示してください。

TaskカードにはPrompt 1と同じく以下を表示してください。

- Subject
- ActivityDate

ActivityDateが設定されていない場合でも画面が壊れないようにしてください。

Salesforce GraphQLのレスポンスは、必要に応じてPrompt 1で使用しているTask型へ変換し、既存のReactコンポーネントをできる限りそのまま利用してください。

## ローディング・空データ・エラー

Salesforce Taskを取得している間は、データを取得中であることが分かる表示をしてください。

Taskが0件の場合は、エラー扱いにはせず、表示するTaskがないことが分かる状態にしてください。

データ取得に失敗した場合はアプリ全体をクラッシュさせず、簡潔なエラーメッセージを表示してください。

## Drag & Drop

Prompt 1で実装した `@dnd-kit/react` によるDrag & Dropをそのまま維持してください。

新しいDrag & Dropライブラリへの変更は行わないでください。

旧APIである以下のパッケージへ変更しないでください。

- `@dnd-kit/core`
- `@dnd-kit/sortable`
- `@dnd-kit/utilities`

同一セル内でのTaskの並び替え機能は必要ありません。

Taskカードを別のセルへDropした場合、Drop先セルに対応するStatusとPriorityを取得してください。

例えば、

High / Not Started

にあるTaskを、

Normal / In Progress

へDropした場合は、そのTaskの

- Statusを `In Progress`
- Priorityを `Normal`

へ変更してください。

横方向への移動ではStatus、縦方向への移動ではPriority、斜め方向への移動ではStatusとPriorityの両方が変更されるようにしてください。

## Salesforce Taskの更新

Taskカードを別のセルへDropしたときは、React上の表示だけではなく、SalesforceのTaskレコードも更新してください。

Salesforceレコードの更新にはData SDKとGraphQL mutationを使用し、`dataSdk.graphql?.mutate()` を使用してください。

DropされたTaskのIdを指定し、Drop先に応じて以下の項目を更新してください。

- Status
- Priority

StatusとPriorityのどちらも変更されていない場合は、不要なmutationを実行しないでください。

Salesforce Taskの更新は、ユーザーがTaskをDrag & Dropした場合だけ実行してください。

更新が成功した場合はTaskを新しいセルに表示してください。

更新に失敗した場合は、Taskを更新前のStatusとPriorityへ戻し、更新に失敗したことを簡潔に表示してください。

同じTaskについて更新処理が実行中の場合、連続したDrag & Dropによって状態が壊れないようにしてください。

過度に複雑な状態管理ライブラリは追加せず、Reactの標準的なstate管理を使用してください。

更新成功後は、必要に応じてData SDKのquery結果をrefreshするなどして、Salesforceに保存された実際のデータと画面の状態が一致するようにしてください。

画面を再読み込みした場合にも、Salesforceに保存されている最新のStatusとPriorityに基づいてTaskが正しいセルへ表示されるようにしてください。

## 既存実装を維持するための条件

- Prompt 1で作成したSalesforce Multi-FrameworkのReact Internal Appをそのまま更新してください。
- 新しいアプリを作成しないでください。
- UIBundle名を変更しないでください。
- CustomApplication名を変更しないでください。
- アプリ名を変更しないでください。
- Prompt 1で作成した5列×3行のKanbanを維持してください。
- 横軸 = Status、縦軸 = Priorityの構造を維持してください。
- TaskカードのSubjectとActivityDate表示を維持してください。
- `@dnd-kit/react` を引き続き使用してください。
- ReactとTypeScriptの構成を維持してください。
- Salesforce Multi-Frameworkのプロジェクト構成を維持してください。
- Prompt 1の全体的なデザインと操作性を必要以上に変更しないでください。

今回の目的は、Prompt 1で作成したモック版KanbanのデータソースをSalesforce Taskへ変更することです。

不要な以下の機能は追加しないでください。

- 新しいページ
- ナビゲーション
- Task作成画面
- Task編集フォーム
- Task削除
- 検索
- フィルター
- ソートUI
- チャート
- ダッシュボード
- モーダル

## Salesforceの権限

現在のユーザーがReactアプリとTaskデータへアクセスできる状態にしてください。

Prompt 1で作成・デプロイされたPermission Setがある場合は、それを維持してください。

必要に応じて、Taskに対する以下の権限を既存のPermission Setへ追加してください。

- Taskの読み取り
- Statusの読み取りと更新
- Priorityの読み取りと更新
- Subjectの読み取り
- ActivityDateの読み取り

必要以上に強い権限を付与しないでください。

組織全体のセキュリティ設定、Profile、共有設定などを勝手に変更しないでください。

プロジェクト内のPermission Setでは安全に解決できない権限上の問題がある場合は、その内容を報告してください。

## production build

実装が完了したら、Prompt 1と同じUIBundleについてproduction buildを実際に実行してください。

現在の `package.json` とscriptsを確認し、このプロジェクトで定義されているbuild scriptを使用してください。

通常は以下に相当する処理です。

`npm run build`

TypeScriptエラー、GraphQL関連のエラー、dependencyエラー、buildエラーなどが発生した場合は、そのままデプロイへ進まないでください。

エラーの原因を確認して修正し、production buildが正常に成功する状態にしてください。

UIBundleのデプロイ対象となるbuild成果物が正常に生成されたことを確認してください。

## Salesforceへの再デプロイ

production buildが成功したら、Prompt 1で使用したものと同じ、現在Agentforce Vibesで認証済みかつデフォルトとして設定されているSalesforce組織へ再デプロイしてください。

別のSalesforce組織へ接続先を変更しないでください。

新しいCustomApplicationを作成せず、Prompt 1でデプロイした既存のアプリを更新してください。

以下を必要に応じて含めてデプロイしてください。

- 更新されたUIBundle
- 既存のCustomApplication
- 更新されたPermission Set
- その他、このReact Internal Appに必要な関連メタデータ

Salesforce DXプロジェクトの構造を確認し、現在のプロジェクトに適したSalesforce CLIのデプロイ処理を実際に実行してください。

必要であれば以下に相当する処理を実行してください。

`sf project deploy start --source-dir force-app`

単に実行すべきコマンドを説明するだけではなく、Agentforce Vibes自身がデプロイを実行してください。

デプロイに失敗した場合はエラー内容を確認してください。

今回変更したReactコード、TypeScript、Data SDK、GraphQL、npm dependency、UIBundle、Permission Setなどに起因する問題であれば修正し、再度production buildとデプロイを実行してください。

Salesforce組織の設定や管理権限など、プロジェクトのコードから安全に修正できない問題の場合は、組織設定を勝手に変更せず、必要な対応を明確に説明してください。

## アプリへのアクセス

デプロイが成功したら、デプロイ結果が成功していることを確認してください。

以下が正常にデプロイされていることを確認してください。

- 更新されたUIBundle
- 既存のCustomApplication
- 必要なPermission Set

Prompt 1でPermission Setが現在のユーザーへ割り当てられている場合は、その状態を維持してください。

React Internal AppはPrompt 1と同じアプリ名で、SalesforceのApp Launcherから利用できる状態にしてください。

## 最後の報告

すべての作業が完了したら、長い説明はせず、以下だけを簡潔に報告してください。

- Salesforce Taskとの接続結果
- SalesforceからTaskを取得している主な実装箇所
- Drag & Drop後にStatusとPriorityを更新している主な実装箇所
- production buildの結果
- Salesforceへの再デプロイ結果
- デプロイした既存アプリ名
- App Launcherからアプリを開くために参加者が行う操作

デプロイや実装が成功していない場合は、成功したかのように報告しないでください。

実際にSalesforce画面上で確認できていない動作についても、確認済みであるかのように報告しないでください。

参加者がApp Launcherからアプリを開いた後、以下を確認できる状態にしてください。

- SalesforceのTaskがKanbanに表示される
- Taskを横方向へDrag & DropするとStatusが更新される
- Taskを縦方向へDrag & DropするとPriorityが更新される
- Taskを斜め方向へDrag & DropするとStatusとPriorityの両方が更新される
- Salesforce画面を再読み込みしても変更結果が維持される
