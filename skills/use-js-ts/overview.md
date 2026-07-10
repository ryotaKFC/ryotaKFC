# ECMAScript Modules（ES Modules）

プログラムをモジュールという単位に分割する 機能 のこと

- 1ファイル === 1モジュール
- スコープはモジュールごと
- 関数や変数をimport/export可能

## named export/import

変数名、関数名にexportをすると、その名前でimportできる機能のこと

## default export

- 名前をつけずにexportできる
- import時に任意の名前を設定できる
- defaultは1モジュール1つ

## Narrowing

緩い型を狭い型に絞り込むテクニック
ただtypeofでの型絞り込みは関数スコープ完結

```ts
function hoge(padding: string | number) {
    if (typeof padding === "number") {
        // このブロックではpaddingはnumber
    } else if (typeof padding === "string") {
        // このブロックではpaddingはstring
    }
}
```

## is

isを使うとコンパイラにこれはこの型だよって教えられる
typeofでstringに絞り込んで、isで返り値はstringだと明記してあげれば呼び出し側もstringに解釈される

## in

二つの意味がある構文

オブジェクトが特定のプロパティを持つか判定するもの

```js
const example = (fishOrBard: Fish | Bard) => {
  // fishOrBardは'fly'というプロパティを持っているかどうかの判定
  if ("fly" in fishOrBard) {
    console.log(fishOrBard.fly); // Bard型として推論される
  } else {
    console.log(fishOrBard.swim); // Fish型として推論される
  }
};
```

## as

推論された型を上書きできる構文
推論された型が期待通りでない場合に使う
けど基本利用はやめるべき（実行時エラーにつながる）

## any/unknown

- any
プロパティアクセスが型エラーにならない
- unknown
プロパティアクセスが型エラーになる

## 使い所

外部APIのレスポンスなど、型が不明な時にナローイングして使う

## Generics

型を引数として渡せる機能

```ts
function getJson<T>(url: string): Promise<T>{
    return fetch(url).then<T>((res) => res.json())
}

// 返り値がUser[]になる
const users = await getJson<User[]>("/api/users")
```
