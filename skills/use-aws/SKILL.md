---
name: use-aws
description: AWSのCloudWatch(ログ・メトリクス・アラート・ChatBot通知)とLambda/serverless.ymlに関する学習メモ。AWSのログ監視・アラート設計やLambdaの制約、serverless.ymlの環境変数設定について話す際に参照する。
---

## CloudWatch

### ログとメトリクスの違い
- ログ ≠ メトリクス
- ログ: 文章やイベントが一行ずつ記録されたもの (例: `print("aaa")` で出力した内容もそのまま拾われる)
- メトリクス: ログを時系列の数値に変換したもの
- CloudWatchでアラートを出せるのはメトリクスに変換した後のみ

### Metric Filter
- ログをちゃんと扱うには Metric Filter を使ってメトリクス化する
- 例: エラー発生時に発火させたい場合 → エラーログをメトリクスに変換し、値が1以上で通知する設定にする

### ChatBot連携
- CloudWatchのアラート(メトリクス発火)をチャットツール(Slack等)で受け取れる
- 簡単なコマンド実行も可能
- ChatBot自体はメトリクスで発火するため、エラーメッセージそのものはチャットに直接送れない
- ただしSlackではChatBotのメッセージにエラー詳細取得用のコマンドボタンが付いており、そこから詳細を確認できる

## Lambda

### Lambdaとは
- サーバーレスな実行環境
- EC2は常時ホスティングが必要で運用が大変だが、Lambdaは必要な時だけ関数を実行するため運用コストが低い
- ホスティングしないため、Websocketや状態の保持、画像の保存などは外部サービスに任せる必要がある
- Lambda内の処理は `return` した時点で凍結される
  - → 呼び出し元に成功レスポンスを返してから裏で非同期処理を続ける、ということが1つのLambdaでは実現困難

### serverless.yml
- AWSの構成を記述するファイル。構成をまとめることで再現性を保つ
- 環境ごとの設定は `env.stg.yml` / `env.prd.yml` に分かれる
- `${prefix:xxxxx}` の形式で値を参照し、prefixは取得元を表す
  - `aws:` デプロイ先AWSアカウント情報から
  - `sls:` Serverless Frameworkから
  - `self:` serverless.yml自身から
  - `param:` env.stg.yml等のenvファイルから
