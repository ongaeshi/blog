---
Title: mrubyでお絵描きプログラミング！10分ではじめるRubyKokuban
Category:
- rubykokuban
- ruby
- mruby
- openframeworks
Date: 2013-12-16T00:04:15+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubykokuban-sketch
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815714530231
---

この記事は[mruby Advent Calendar 2013の16日目](http://qiita.com/ongaeshi/items/99ce7c25d961a57d0633)です。

## はじめに
少し前に[RubyKokuban](http://ongaeshi.hatenablog.com/entry/rubykokuban-release)というものを作りました。

- [ongaeshi/rubykokuban-gem](https://github.com/ongaeshi/rubykokuban-gem)

このライブラリを使うと、簡単にグラフィックやアニメーションを使ったアプリケーションを書くことが出来ます。

## RubyKokubanとは
RubyKokubanは<b>openFrameworksにmrubyを組み込んだもの</b>です。Rubyを使って簡単にグラフィックスやアニメーションを利用したプログラムを書くことが出来ます。

以下のように、(関数定義を除けば)2行で画面に文字と円を表示することが出来ます。

```ruby
def draw
  text 'Hello, rubykokuban!', 100, 100
  circle 350, 100, 50
end
```

![demo-01.png](https://raw.github.com/ongaeshi/rubykokuban-gem/data/images/demo-01.png)

たくさん書いてみます。

```ruby
def setup
  set_window_size 200, 200
  set_background Color::Black
end

def draw
  (1..100).each do
    set_color rand(255), rand(255), 128, 128
    circle rand(200), rand(200), rand(10) + 20
  end
end
```

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215152748.png" alt="f:id:tuto0621:20131215152748p:plain" title="f:id:tuto0621:20131215152748p:plain" class="hatena-fotolife" itemprop="image"></span></p>

動いた！といいたい所ですがdrawが毎F呼ばれているために円が高速に書き換えられてしまいます。 ※ 後で直します

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215152810.gif" alt="f:id:tuto0621:20131215152810g:plain" title="f:id:tuto0621:20131215152810g:plain" class="hatena-fotolife" itemprop="image"></span></p>

RubyKokubanがどんなものなのかは伝わったと思うので、<s>バグは放置して</s>インストール方法を紹介します。

## RubyKokubanを使うための準備
RubyKokubanを使うには以下の手順が必要です。

1. kokuban gem をインストールする
1. RubyKokubanをダウンロードする
1. コードをテキストファイルに書く
1. `kokuban`コマンドを使って実行する

`ruby`コマンドの代わりに`kokuban`コマンドを使う感じです。それではやってみましょう。

### kokuban gem をインストールする
RubyGemsからインストールします。

```
$ gem install kokuban 
```

kokubanコマンドが使えるようになれば成功です。

```
$ kokuban
kokuban 0.2.0

Commands:
  kokuban exec [input_file]  # Execute rubykokuban file
  kokuban help [COMMAND]     # Describe available commands or one specific command
  kokuban install [options]  # Install RubyKokuban.app
  kokuban list               # Display installed version
  kokuban uninstall          # Uninstall rubykokuban from the local repository

Options:
  -h, [--help]  # Help message
```

### RubyKokubanをダウンロードする
```
$ kokuban install --latest
```

各プラットホーム用にビルドされたクライアントがダウンロードされます。現在は<b>Mac</b>のみ対応しています。

※ openFrameworks自体はWindows, Linuxに対応しているので[ongaeshi/rubykokuban-osx](https://github.com/ongaeshi/rubykokuban-osx)を参考にすれば移植は可能だと考えています。誰かやってくれたら嬉しいなぁ。

### コードを書く
以下のコードを<i>hello.rb</i>という名前で保存します。(テキストファイルは好きなもので構いません)

```ruby
def setup
  set_window_size 300, 300
  set_background Color::White
end

def draw
  set_fill

  set_color Color::Red, 128
  rect 70, 70, 60, 60

  set_color Color::Green, 128
  circle 200, 100, 30

  set_color Color::Blue, 128
  triangle 150 - 30, 200, 150 + 30, 200, 150, 200 + 52
end
```

### 実行する
`kokuban exec`コマンドで実行します。3つの図形が出たら成功です。

```
$ kokuban exec hello.rb
```

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215153105.png" alt="f:id:tuto0621:20131215153105p:plain" title="f:id:tuto0621:20131215153105p:plain" class="hatena-fotolife" itemprop="image"></span></p>

### 調整する
楕円を追加してみます。

```diff
def draw
  .
  .
  set_color Color::Blue, 128
  triangle 150 - 30, 200, 150 + 30, 200, 150, 200 + 52
+
+  set_color Color::Black
+  set_no_fill
+  ellipse 150, 150, 200, 270
```

ここでポイントです！

書き換えたら閉じずに<b>ウィンドウにフォーカスを合わせてCtrl+R</b>を押します。
自動リロードされるので開発がとても快適になります。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215153120.png" alt="f:id:tuto0621:20131215153120p:plain" title="f:id:tuto0621:20131215153120p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## なにか書いてみよう
### 最初のサンプルをちゃんとする
最初のサンプルに戻りましょう。

```ruby
def setup
  set_window_size 200, 200
  set_background Color::Black
end

def draw
  (1..100).each do
    set_color rand(255), rand(255), 128, 128
    circle rand(200), rand(200), rand(10) + 20
  end
end
```

円が高速に書き換えられてしまう問題を修正します。
setupで事前に描画用のデータを作りdrawはそれを表示するだけに改造してみます。

```ruby
class Shape < Struct.new(:x, :y, :radius, :color); end

def setup
  set_window_size 200, 200
  set_background Color::Black
  set_fill

  @shapes = []
  (1..100).each do
    @shapes << Shape.new(rand(200), rand(200), rand(10) + 20,
                         Color.new(rand(255), rand(255), 128, 128))
  end
end

def draw
  @shapes.each do |d|
    set_color d.color
    circle d.x, d.y, d.radius
  end
end
```

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215152748.png" alt="f:id:tuto0621:20131215152748p:plain" title="f:id:tuto0621:20131215152748p:plain" class="hatena-fotolife" itemprop="image"></span></p>

コードは少し長くなりましたが大分見通しのよいコードになりました。

- Shapeという描画用データを保持するための型が定義
- @shapes配列が生成情報を管理
- draw関数は配列のデータを受け取ってそのまま表示

### インタラクティブにする
Rubyの表現力の高さを活かして少し複雑な動きを書いてみましょう。

マウスカーソルがウィンドウにある時は円がカーソルに近づき、ウィンドウから出たら元の位置に戻るようなデモを書いてみます。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131215/20131215152953.gif" alt="f:id:tuto0621:20131215152953g:plain" title="f:id:tuto0621:20131215152953g:plain" class="hatena-fotolife" itemprop="image"></span></p>

元の位置に保存するためにShape#ox, Shape#oyメンバを追加します。update関数を追加してカーソルに追従したり元の位置に戻る処理を書きます。

```ruby
class Shape < Struct.new(:x, :y, :radius, :color, :ox, :oy); end

def setup
  set_window_size 200, 200
  set_background Color::Black
  set_fill

  @shapes = []
  (1..100).each do
    s = Shape.new(rand(200), rand(200), rand(10) + 20,
                  Color.new(rand(255), rand(255), 128, 128))
    s.ox = s.x
    s.oy = s.y
    @shapes << s
  end
end

def update
  @shapes.each do |d|
    if 0 < Input.mouse_y && Input.mouse_y < 200 &&
        0 < Input.mouse_x && Input.mouse_x < 200 
      d.x += (Input.mouse_x - d.x) * 0.08
      d.y += (Input.mouse_y - d.y) * 0.08
    else
      d.x += (d.ox - d.x) * 0.2
      d.y += (d.oy - d.y) * 0.2
    end
  end
end

def draw
  @shapes.each do |d|
    set_color d.color
    circle d.x, d.y, d.radius
  end
end
```

繰り返しの処理をブロック構文ですっきりと書けるのはやはりいいですね。

### もっと書きたい
以下にサンプルコードがあります。

- [ongaeshi/rubykokuban-sample](https://github.com/ongaeshi/rubykokuban-sample)

シューティングゲームもありますよ。

![mouse-shooting2](https://raw.github.com/ongaeshi/rubykokuban-sample/master/images/mouse_shooting2.gif)

## まとめ
グラフィックを操作するプログラミングはとても楽しいので是非一度触ってみて下さい。最新版では[画像を表示](http://ongaeshi.hatenablog.com/entry/rubykokuban-image)することも出来るようになりました。

### RubyKokubanのメリット
Rubyで書ける以外に何かいいことがあるのでしょうか？

CRubyやPythonでopenFrameworksをラップした場合、openFrameworksはライブラリとして提供され、CRubyやPythonからライブラリにアクセスすることになります。ユーザーからはRubyスクリプトしか見えませんがOSから見るとCRubyとopenFrameworksの二つのアプリケーションが連携していることになります。

mrubyでopenFrameworksを利用する場合<b>mrubyはopenFrameworksの中に隠れます</b>。OSから見るとopenFrameworksしか存在しないように見えます。提供する機能は違いますがOpenCVやBox2Dといった他のopenFrameworksのライブラリと本質的に違いはありません。RubyKokubanはRubyスクリプトを読み込んで実行する<b>openFrameworksアプリケーション</b>なのです。

そのため、以下のようなメリットがあります。

- 単独のアプリケーションとして動作させることが出来る
  - Rubyスクリプトを内部に組み込めば全てC++で書かれたものと違いはなくなる
- 速度が重要な部分やまだ組み込まれていない機能はC++で書く事が出来る

### 参考文献
- [ブラウザでお絵描きプログラミング！ Processing.js 登場！ - IT戦記](http://d.hatena.ne.jp/amachang/20080509/1210355674)
- [まつもとゆきひろ直伝　組込Ruby「mruby」のすべて 総集編](http://tatsu-zine.com/books/mruby)
- [Beyond Interaction　メディアアートのためのopenFrameworksプログラミング入門](http://www.bnn.co.jp/books/3746/) - pdf版が無料でダウンロードで来ます
