
# Reactって

- UIを構築するためのライブラリ
- UIを関数で定義する
- JSXっていうHTML-LIKEな拡張構文を使う

## Reactを使うと嬉しいこと

素のHTML, CSSだと...

- DOM AIで操作する
- マークアップとロジックが離れてる

```html
<input type="text" id="input" />
<h1 id="output">こんにちは！さん</h1>

<script>
  const input = document.getElementById('input');
  const output = document.getElementById('output');
  
  input.addEventListener('input', (e) => {
    output.textContent = `こんにちは！${e.target.value}さん`;
  });
</script>
```

Reactだと...

- UIに必要なものが全て一箇所に収まってる
  - メンテナンスしやすい
- DOM APIを直せず使わずに簡潔に書ける

```jsx
import { useState } from 'react';

export default function App() {
  const [name, setName] = useState('');

  return (
    <div>
      <input type="text" onChange={(e) => setName(e.target.value)} />
      <h1>こんにちは！{name}さん</h1>
    </div>
  );
}
```

### Vertual DOM （仮想DOM

- stateが変わるとReactが新しい仮想のDOMを生成
- 前回との差分を計算して、変わったところだけ実際のDOMに反映

DOM: ドキュメントをどのような構造にしてどのように操作するかを定義したもの
