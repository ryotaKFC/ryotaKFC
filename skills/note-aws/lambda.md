# AWS Lambdaとは？
サーバーレスな仕組み
EC2は常時ホスティングをするため運用がだるい
→ Lambdaなら必要な時だけ関数を実行するため、運用コスト低
ただしホスティングをしないため、Websocketや状態の保持、画像の保存などは外部に任せる必要がある
また、Lambda内の処理はreturnされた時点で凍結される。
→ 一旦呼び出し元に成功レスポンス返して非同期を裏で実行とかは一つのLambdaだと厳しい

# serverless.yml
AWSの構成を記述したファイル
このファイルに構成をまとめることで再現性を保つ
### envの読み取り
.enbは環境によって、env.stg.ymlとenv.prd.ymlに分かれる
```
${aws:xxxxx}
```
先頭のプレフィックスは、どこから値を持ってくるかを表している
- aws: デプロイ先のAWSアカウント情報から
- sls: serverless Framework
- self: serverless.yml自身
- param: env.stg.ymlファイルから
