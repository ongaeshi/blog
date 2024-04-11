---
Title: ' mrubyで簡単にグラフィカル＆インタラクティブなアプリケーションが作れるRubyKokubanを作りました'
Category:
- ruby
- mruby
- cpp
- openframeworks
- C++
- game
Date: 2013-09-09T10:06:07+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubykokuban-release
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318757302071
---

mrubyを使ってグラフィカル＆インタラクティブなアプリケーションが作れるRubyKokubanというものを作りました。
mrubyで何かやりたいなあと思いこつこと書いていたのですが、少し触れるようになってきたので公開します。


```ruby
def draw
  text 'Hello, rubykokuban!', 100, 100
  circle 350, 100, 50
end
```

![hello-demo](https://raw.github.com/ongaeshi/rubykokuban-gem/data/images/demo-01.png)

最小限のコードでゲームのようなものが作れることを目指しています。

## インストール

今はコマンドラインからの実行環境のみ用意されています。対応プラットフォームもOSXのみです。

RubyGemsからインストールします。

```
$ gem install kokuban
```

まずは最新の実行用アプリケーションをインストールしましょう。

```
$ kokuban install --latest
```

インストールされていることを確認します。

```
$ kokuban list
osx(0.0.3)
```

## Hello, RubyKokuban

図形と文字を書いてみましょう。

```ruby
def draw
  text 'Hello, rubykokuban!', 100, 100
  circle 350, 100, 50
end
```

実行します。

```
$ kokuban exec hello.rb
```

![hello-demo](https://raw.github.com/ongaeshi/rubykokuban-gem/data/images/demo-01.png)

動かしてみましょう。

```ruby
def setup
  set_window_size 480, 240
  @x = 0
end

def update
  @x += 1
end

def draw
  text 'Hello, rubykokuban!', 100, 100
  circle @x, 100, 50
end
```

実行します。ウィンドウが開いたままだったらCtrl+R(⌘+Rじゃないので注意)でリロードしてもいいです。

![move-demo](https://raw.github.com/ongaeshi/rubykokuban-gem/data/images/demo-02.gif)

簡単ですね！

## 内部技術の解説

- mruby + openFrameworks で作られています
  - ライブラリではなく「mrubyが組み込まれたアプリケーション」です。
- 将来的に以下のようなことが期待出来ます。
  - 複数のプラットフォームで同じコードが動きます。
  - モバイルプラットフォームでも動かす事が出来ます。
  - スクリプトをバイナリファイルに埋め込むことが出来ます。

## これからやりたいこと

- openFrameworks APIのバインド続き
  - 画像の表示
  - キーボード
  - サウンドなど
- osxの実行用アプリケーションの機能強化
  - 複数ファイルのロード
  - ファイル変更を監視して自動リロード
  - ドラッグ＆ドロップ出来るようにしたり
- 他のプラットフォームで動くように
- サンプルコードの充実

サンプルコード含め、プルリクエストはいつでも歓迎です！

## リソース
- [rubykokuban-gem](https://github.com/ongaeshi/rubykokuban-gem) - コンソールでのインストール＆実行
- [rubykokuban-osx](https://github.com/ongaeshi/rubykokuban-osx) - OSXでの実行用アプリケーション
- [rubykokuban-mruby](https://github.com/ongaeshi/rubykokuban-mruby) - 内部で使っているmruby
- [rubykokuban-sample](https://github.com/ongaeshi/rubykokuban-sample) - サンプルコード

続きです → <span style="font-size: 150%">[画像が表示出来るようになりました。](http://ongaeshi.hatenablog.com/entry/rubykokuban-image)</span>

