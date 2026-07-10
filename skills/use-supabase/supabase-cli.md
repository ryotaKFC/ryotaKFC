# supabaseの罠
- config.tomlにsupabaseの設定が書かれてる
- supabase側のデフォルトだと開発環境のURLが127.0.0.1になってる
- ただしNext.js側の設定だと開発環境はlocalhostになってる
- なのでどっちかに統一しないとOAuth周りでグダる（localhostと127.0.0.1が別扱いになってる）
```
// config.tomlの方を変える場合
site_url = "http://localhost:3000"
external_url = "http://localhost:54321/auth/v1"
additional_redirect_urls = ["https://localhost:3000"]
```
