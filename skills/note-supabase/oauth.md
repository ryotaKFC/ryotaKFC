# supabase authを使う場合、アプリ側でエンドポイントは用意しなくていい
Googleとかでログイン後supabase側が
認可コード取得→トークン交換→DB登録
までやってくれるので、アプリ側はわざわざエンドポイントを用意せずとも、getUser()みたいな関数だけでログイン情報を取得できる

# supabaseでOAuth実装する時、認可コードの引換方法は二種類ある。
## 普通にアプリ側でエンドポイントを立てる
エンドポイント内で認可コードを引換てCookieに保存してセッションを確立する
まあよく見るであろう処理方法

## Supabaseに全て任せる
supabaseを使う場合、エンドポイントがなくてもセッションの確立をいい感じにやってくれる
※しかもImplicit Grant Flowではなく、ちゃんとPKCEフロー
### ログイン前
1. signInWithGoogle実行時に、supabase/ssrが内部でcode_verifierを生成
2. それをハッシュ化してcode_challengeを作成
3. code_verifierをcokkieに保存
4. code_challengeの方をSupabaseに渡してsupabaseがGoogleにjリダイレクト
### Google認証後
1. googleがパスに認可コード（`?code=xxxx`）を含めて返す
### ログイン後
1. supabase clientがURLを読み取り、`?code=`の値を読み取る
2. Cookieからさっきのcode_verifierを読み取る
3. 認可コードとcode_verifierをセットでsupabaseに送る
4. supabase側がcode_verifierをハッシュ化したものと最初に受け取ったcode_challengeが一致するか検証
つまりアプリ側はsupabaseとのやり取りだけを意識すればいいように作られてる