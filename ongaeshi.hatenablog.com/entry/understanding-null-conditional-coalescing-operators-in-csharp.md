---
Title: C# の null 条件演算子と null 合体演算子の組み合わせを理解する
Category:
- C#
Date: 2024-04-25T00:41:24+09:00
URL: https://ongaeshi.hatenablog.com/entry/understanding-null-conditional-coalescing-operators-in-csharp
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189101207313
---

C# における `if (obj?.Foo?.Bar ?? false)` のような [null 条件演算子](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)と [null 合体演算子](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/operators/null-coalescing-operator)を組み合わせた式が読みにくくて苦手だった。

```cs
if (obj?.Foo?.Bar ?? false) {
    // obj.Foo.Bar の途中に null が存在しない、
    // かつ obj.Foo.Bar == true の場合に実行される。
}
```

特に `if (!obj?.Foo?.Bar ?? true)` のような真偽が反転するような書き方だと頭がこんがらがってしまう。

```cs
if (!obj?.Foo?.Bar ?? true) {
    // obj.Foo.Bar の途中に null が存在する、
    // または obj.Foo.Bar == false のときに実行される。（ややこしい・・）
}
```

そもそもなぜ `if (obj?.Foo?.Bar)` では駄目なのか？

```cs
if (obj?.Foo?.Bar) {
    // コンパイルエラー
}
```

途中に null 条件演算子があるため式全体の戻り値の型が `bool?` として評価されてしまうのが原因だ。C# では [Ruby などの言語と違って](https://rubyonbrowser.ongaeshi.me/?q=JYMwBAFAdsA2CUAoMYAOYBENYcQU1gGc9k1MoB7AFzG1zygBMg) null が if 文の偽として振る舞うことが禁止されている。そのため Bar が bool を返すプロパティであってもコンパイルエラーとなってしまう。

そこで null 合体演算子の出番となる。`if (obj?.Foo?.Bar ?? false)` のように null の場合に代わりの値を明示することで式全体の戻り値は `bool`` となり無事コンパイルを通過することができる。

```cs
if (obj?.Foo?.Bar ?? false) {
    // obj.Foo.Bar の途中に null が存在しなければその結果を、そうでなければ false
}
```

少しずつ理解を進めていったあるとき、同僚が `int i = obj?.Foo?.Bar ?? 0` のような書き方しているのを見た。

```cs
// obj.Foo.Bar の途中に null が存在しなければその結果を、そうでなければ 0
int i = obj?.Foo?.Bar ?? 0; 
```

なるほどつまり null 合体演算子 は **swtch 文の default や switch 式の `_` などデフォルト値のように振る舞っている**と考えればいいのだと気がついた。

ここで2つ目の式に戻る。`if (!obj?.Foo?.Bar ?? true)` の null 合体演算子が null 条件演算子 のデフォルト値として振る舞っていると考えると、この式はすでに一度別の法則で展開されていることに気がつく。そう **ド・モルガンの法則** だ。

```cs
if (!(obj?.Foo?.Bar ?? false)) {
    // 「obj.Foo.Bar の途中に null が存在しなければその結果を、そうでなければ false」の結果を反転する
}
```

つまり `if (!obj?.Foo?.Bar ?? true)` は `if (!(obj?.Foo?.Bar ?? false))` の括弧を一度展開した状態だったのだ、やっと理解できた！
