---
Title: Caseninja - 入力されたテキストをチェイン、スネーク、キャメル、パスカルケースにまとめて変換
Category:
- ruby
Date: 2015-08-30T01:20:09+09:00
URL: https://ongaeshi.hatenablog.com/entry/caseninja
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415119459040
---

[https://twitter.com/ongaeshi/status/637461911756402688:embed#最近メンテナンスフェーズ(バグ修正したり、Webサイト作ったり)が多くて飽きてきたので、新機能を作ろうという気分になった。個人開発者には自分のモチベーション管理重要。]

久しぶりに新しいものを作りたくなりました。

## これは何？

[ongaeshi/caseninja](https://github.com/ongaeshi/caseninja)

コーディング中にGoogle翻訳を使って名前を決めることがよくあるのですが、"This is pen"を決めたあとに

- ファイル名なら`this_is_pen.rb`
- クラス名なら`ThisIsPen`
- 変数名なら`thisIsPen`や`this_is_pen`
- 定数名なら`THIS_IS_PEN`
- Lisp書いてるときは`this-is-pen`

のように規約によって大文字、小文字、区切り文字を変える必要がありました。それらの手間を減らすためのツールです。

## インストール

```
$ gem install caseninja
```

`caseninja`コマンドが使えるようになったら成功です。他のgemには依存していないので簡単にインストールできます。

```
$ caseninja
Usage: caseninja [options] args
        --chain                      Convert to chain case
        --snake                      Convert to snake case
        --camel                      Convert to camel case
        --pascal                     Convert to pascal case
        --upchain                    Convert to upper chain case
        --upsnake                    Convert to upper snake case
```

## 使い方 
空白区切りの文章を渡すとよく使うケースにまとめて変換します。

```
$ caseninja "hello world"
hello-world                                  # chain
hello_world                                  # snake
helloWorld                                   # camel
HelloWorld                                   # pascal
HELLO-WORLD                                  # upchain
HELLO_WORLD                                  # upsnake
```

空白区切りじゃなくても上手く変換してくれます。

```
$ caseninja helloWorld
hello-world
.

$ caseninja HELLO_WORLD
hello-world
.
```

特定のケースにのみ変換することもできます。

```
$ caseninja fooBarToBaz --snake
foo_bar_to_baz

$ caseninja fooBarToBaz -s -p
foo_bar_to_baz
FooBarToBaz
```

長い文章でもOKです。

```
$ caseninja "What does the Japanese word Dattebayo mean?"
what-does-the-japanese-word-dattebayo-mean?
what_does_the_japanese_word_dattebayo_mean?
whatDoesTheJapaneseWordDattebayoMean?
WhatDoesTheJapaneseWordDattebayoMean?
WHAT-DOES-THE-JAPANESE-WORD-DATTEBAYO-MEAN?
WHAT_DOES_THE_JAPANESE_WORD_DATTEBAYO_MEAN?
```

ファイル名、モジュール名、型名、定数名、変数名、ブランチ名・・・と作業中にケースの切り替える回数はそれなりに多いのではないかと思います。よかったらお使い下さい。
