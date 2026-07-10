---
name: note-supabase
description: Supabase(認証・CLI設定)に関する学習メモ。Supabase AuthでのOAuth/PKCE実装や認可コード引換の方式選択、Next.js等と組み合わせたローカル開発環境のconfig.toml設定でハマった際にClaudeが参照する。
---

## Supabase Authの仕組み

- Supabase Authを使う場合、アプリ側で認可コード交換用のエンドポイントを自前で用意する必要はない
- Googleなど外部プロバイダでログインすると、Supabase側が以下を代行してくれる
  - 認可コード取得 → トークン交換 → DBへのユーザー登録
- アプリ側は `getUser()` のような関数を呼ぶだけでログイン情報を取得できる

## OAuthの認可コード引換方法(2種類)

### 1. アプリ側でエンドポイントを立てる

- エンドポイント内で認可コードを引き換え、Cookieに保存してセッションを確立する
- よく見る一般的な処理方法

### 2. Supabaseに全て任せる

- エンドポイントがなくてもSupabaseがセッション確立をいい感じにやってくれる
- しかもImplicit Grant Flowではなく、ちゃんとPKCEフローになっている
- アプリ側はSupabaseとのやり取りだけを意識すればよい設計

#### PKCEフローの流れ

- **ログイン前**
  1. `signInWithGoogle` 実行時に、`supabase/ssr` が内部で `code_verifier` を生成
  2. それをハッシュ化して `code_challenge` を作成
  3. `code_verifier` をCookieに保存
  4. `code_challenge` をSupabaseに渡し、SupabaseがGoogleにリダイレクト
- **Google認証後**
  1. Googleがパスに認可コード(`?code=xxxx`)を含めて返す
- **ログイン後**
  1. Supabase clientがURLの `?code=` の値を読み取る
  2. Cookieから `code_verifier` を読み取る
  3. 認可コードと `code_verifier` をセットでSupabaseに送る
  4. Supabase側が `code_verifier` をハッシュ化したものと、最初に受け取った `code_challenge` が一致するか検証

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
