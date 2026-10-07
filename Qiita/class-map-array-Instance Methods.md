# 今日はPaizaで、学級委員としてクラスメイトのデータを年齢順に並び替える任務を任されました(￣^￣)ゞ
昨日公開するつもりが…寝ちゃってた(´;ω;｀)（毎日連続記録が…）


<br>

# 問題概要

- 名前・年齢・誕生日・出身地のデータをUserクラスにまとめる

- それを年齢順に並べ替える

- 並び替えた順に出力する

<br>

入力例:

    1
    koko 23 04/10 tokyo

出力例:

    koko 23 04/10 tokyo

<br>

https://paiza.jp/works/mondai/class_primer/class_primer__sort

<br><br><br><br><br>

# ✅ OK例：
```js
const rl = require('readline').createInterface({ input: process.stdin });

const lines = [];

// 標準入力を1行ずつ lines にためる
rl.on('line', (input) => {
  lines.push(input);
});

// 入力終了後に実行
rl.on('close', () => {
  const N = Number(lines[0]); // 1行目：人数

  // User クラス（設計図）を定義
  class User {
    constructor(nickname, old, birth, state) {
      this.nickname = nickname; // 名前
      this.old = old;           // 年齢
      this.birth = birth;       // 誕生日
      this.state = state;       // 出身地
    }

    // 出力用インスタンスメソッド：User{...} ではなく指定フォーマット
    format() {
      return `${this.nickname} ${this.old} ${this.birth} ${this.state}`;
    }
  }

   // 入力から User インスタンスを作って users に入れる
  const users = [];
  for (let i = 1; i <= N; i++) {
    const [nickname, old, birth, state] = lines[i].split(' ');
    users.push(new User(nickname, old, birth, state));
  }

  // 年齢で昇順ソート
  const sort_old = users.sort((a, b) => a.old - b.old);


  // 1人ずつ format を呼んで出力
  sort_old.forEach(user => console.log(user.format()));
});
```

1️⃣ クラスでデータをまとめる
- `User` は「名前・年齢・誕生日・出身地」という情報の塊を 1 つの形にまとめる設計図。
- `constructor` でデータをセット。
- `format` は 出力フォーマット を統一する便利メソッド。

<br>

2️⃣ `new` でインスタンスを作って配列に格納

- `new User(…)` で インスタンス化。
- 配列 `users` は「`User` のインスタンスの集合」。

<br>

3️⃣ `sort` で並び替え

- `users.sort((a, b) => a.old – b.old)`
→ `a` と `b` は `User` インスタンスだから `a.old` で比較。
- `sort` は 元の配列を書き換える のがポイント。

4️⃣ `.forEach()` と `toString()` で出力

- `console.log(user.format)` で出力形式を統一。
- 直接 `${user.nickname} … ` と書いても同じ結果だけど、`format` を使うと 出力ルールの変更が1か所で済む。

<br><br><br><br><br>

# ✅ 改良ポイント

- `Number()` を `constructor` 内で行う → 入力時に数字変換を忘れにくい

- `for` を `map` に置き換え → 宣言と配列生成を一発で

- ソート後に新しい変数に入れずに、直接 `forEach` に流す

<br><br>

## ✨ 改良版コード
```js
const rl = require('readline').createInterface({ input: process.stdin });

const lines = [];

// 標準入力を配列にためる
rl.on('line', (input) => {
  lines.push(input);
});

rl.on('close', () => {
  const N = Number(lines[0]); // 1行目：人数

  // Userクラス（設計図）
  class User {
    constructor(nickname, old, birth, state) {
      this.nickname = nickname;
      this.old = Number(old); // 数字に変換
      this.birth = birth;
      this.state = state;
    }

    //インスタンスメソッド
    format() {
      return `${this.nickname} ${this.old} ${this.birth} ${this.state}`;
    }
  }

  // mapでまとめてUserインスタンス生成
  const users = lines.slice(1, N + 1).map(line => {
    const [nickname, old, birth, state] = line.split(' ');
    return new User(nickname, old, birth, state);
  });

  // 年齢で昇順ソートして即出力
  users
    .sort((a, b) => a.old - b.old)
    .forEach(user => console.log(user.format()));
});
```

- `map` を使用 `for` よりスッキリ、意図がわかりやすい
→⚠️ブロックで `{}` を使った場合、戻り値は `return` で明示しないと `undefined` になる
- `.slice(1, N + 1)`： 必要な行だけに絞るので安全
- `format()` ：出力を統一しつつ変更しやすい
- `sort().forEach()`： 並び替えてすぐに出力で無駄がない

<br><br><br><br>

# 🗒️まとめ

- `map` と `class` の組み合わせは相性◎
- `map` を使えば宣言と配列生成が同時にできる！
- メソッドチェーンで、データ生成 → ソート → 出力 が流れるように読めるように！

<br><br><br>[僕の失敗談(´;ω;｀)と解決法🐈](https://paizabeginner.wordpress.com/about/)

<br><br><br><br><br><br>

# 💡おまけ:インスタンスメソッド
## 1️⃣ 「インスタンスメソッド」とは？

簡単に言うと：


「クラスから作ったインスタンスが使うメソッド（関数）」

という意味。

<br><br>

## 2️⃣ 例を見てみよう
```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    console.log(`Hello, I'm ${this.name}!`);
  }
}

const user1 = new User("mako");
user1.sayHello();  // Hello, I'm mako!
```
    
- `sayHello()` ← これが インスタンスメソッド
- `user1` は `User` クラスから作った インスタンス
- `user1.sayHello()` ← インスタンスが自分専用の `sayHello` を使う

<br><br>

## 3️⃣ インスタンスメソッドの役割

- インスタンスの持つデータ（プロパティ）を使って処理する
- `this` を使って、自分自身のデータにアクセスする

<br>

例：
```js
sayHello() {
  console.log(`Hello, I'm ${this.name}!`);
}
```
ここで `this.name` は
「このインスタンス（`user1` とか `user2`）が持っている `name` の値」という意味

<br><br>

## 4️⃣ どうして「インスタンスメソッド」と言うの？

`sayHello` は `User` という型そのものが持っているのではなく、
`User` から作ったインスタンス（`user1`, `user2`, `…`）が使える メソッドだから。

<br><br>

## 5️⃣ 「クラスメソッド（静的メソッド）」との違い

JavaScript では、`static` を付けると
インスタンスではなく、クラスそのものが使うメソッド になる。
```js
class User {
  static sayHi() {
    console.log("Hi from User class");
  }
}

User.sayHi(); // ← クラスから直接呼ぶ
```
<br>

これは インスタンスには使えない：
```js
const u = new User();
u.sayHi(); // エラー！
```


<br><br>

クラス自体に属していて、インスタンスを作らなくても呼び出せる
```js
class Calculator {
  // 静的メソッド
  static add(a, b) {
    return a + b;
  }
  
  // インスタンスメソッド（普通のメソッド）
  multiply(a, b) {
    return a * b;
  }
}

// 静的メソッドはクラス名から直接呼べる
console.log(Calculator.add(3, 5));  // 8

// インスタンスを作らないと呼べない
const calc = new Calculator();
console.log(calc.multiply(3, 5));  // 15

// 静的メソッドはインスタンスからは呼べない（エラーになる）
console.log(calc.add(3, 5)); // エラー
```

<br><br>

## 6️⃣ まとめ表
| メソッド           | どこから呼ぶ？      | 何を操作？                   |
|--------------------|---------------------|------------------------------|
| インスタンスメソッド | インスタンスから呼ぶ | そのインスタンスのデータ     |
| 静的メソッド (static) | クラスから直接呼ぶ   | クラス全体で共有する処理やユーティリティ |

<br><br>

## 7️⃣ ポイント！

- `this` はインスタンスメソッドの中では そのインスタンス自身 を指す。
- インスタンスメソッドを作ると、オブジェクトごとに違う値を使って動かせる！
- `static` を付けると、インスタンスには紐づかない。



<br><br>

(8️⃣ 今回のコード)
```js
format() {
  return `${this.nickname} ${this.old} ${this.birth} ${this.state}`;
}

~~~

console.log(user.format());
```
  
- `format()` は `User`クラスのインスタンス（ユーザーオブジェクト）に含まれる情報を「見やすい文字列の形に整えて”返す”」ためのメソッド 。
    
- `this` は 「このメソッドを呼び出したオブジェクト自身」 を指し、値を取り出すためのキーワード
    
- `${}` は文字列の中に変数の値を入れるための便利な構文




