---
name: note-supabase
description: Supabase(認証・CLI設定)に関する学習メモ。Supabase AuthでのOAuth実装や、Next.js等と組み合わせたローカル開発環境のconfig.toml設定でハマった際にClaudeが参照する。
---

## Supabase Authの仕組み

- Supabase Authを使う場合、アプリ側で認可コード交換用のエンドポイントを自前で用意する必要はない
- Googleなど外部プロバイダでログインすると、Supabase側が以下を代行してくれる
  - 認可コード取得 → トークン交換 → DBへのユーザー登録
- アプリ側は `getUser()` のような関数を呼ぶだけでログイン情報を取得できる

## ローカル開発でのURL不一致の罠

- Supabaseの設定は `config.toml` に記載されている
- Supabase側のデフォルトでは開発環境のURLが `127.0.0.1`
- 一方、Next.js側の開発環境は `localhost` がデフォルト
- `localhost` と `127.0.0.1` は別オリジン扱いされるため、どちらかに統一しないとOAuth周りで認証が通らずグダる

```toml
# config.toml側をlocalhostに統一する場合の例
site_url = "http://localhost:3000"
external_url = "http://localhost:54321/auth/v1"
additional_redirect_urls = ["https://localhost:3000"]
```
