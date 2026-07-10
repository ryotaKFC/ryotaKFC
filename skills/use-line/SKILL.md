---
name: use-line
description: LINE Messaging APIのWebhookに関するタイムアウト制約の学習メモ。LINE BotのWebhook実装やLambdaとの組み合わせを検討・実装する際に参照する。
---

参照: https://developers.line.biz/ja/reference/messaging-api/#common-specifications

## Webhookのタイムアウト制約
- LINE Webhookからリクエストを受けてから2秒以内にレスポンスを返さないと、ユーザー側にはエラー表示になる
  - 裏側の処理が成功していてもUXは悪くなる
- 重い処理を行う場合は、先に成功レスポンスをLINE Webhookに返し、メインの処理は非同期で行うのが良さそう

## Lambdaとの組み合わせの注意点
- Lambdaは`return`した瞬間に処理が凍結される([[use-aws]]参照)ため、「先にレスポンスを返して裏で非同期処理を続ける」が1つのLambdaでは実現できない
- そのため、レスポンス用とメイン処理用で2つ程度Lambdaを用意する構成が必要になりそう
