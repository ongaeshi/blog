---
Title: ' iPhoneでRubyとopenFrameworksを使ってグラフィックプログラミングができるofrubyを作りました'
Category:
- ofruby
- ruby
- openFrameworks
- mruby
- ios
- iphone
Date: 2014-08-28T03:03:19+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-released
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815731436778
---

夏休みに作っていたものが先日App Storeの申請を通過したので紹介します。

ofrubyはiPhoneやiPod touch, iPad 上で簡単にグラフィックプログラムを書くことが出来るアプリです。プログラムの記述、実行、デバッグ、管理を<b><span style="color: #cc0000">全てiPhone上</span></b>で行うことが出来るのが特徴です。

ダウンロードはこちらから行うことが出来ます。(無料です)

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

※ ofrubyは以前OSX用に作っていた[RubyKokuban](http://ongaeshi.hatenablog.com/entry/rubykokuban-sketch)のiOS版です。今後はOSX版の方もofrubyという名前に統一していく予定です。

## 使い方
アプリをダウンロードしたら起動します。

<img src="http://ofruby.ongaeshi.me/images/ofruby-00.png" width="320" height="480">

ファイル一覧。

<img src="http://ofruby.ongaeshi.me/images/ofruby-01.png" width="320" height="480">

ファイルを作成します。

<img src="http://ofruby.ongaeshi.me/images/ofruby-02.png" width="320" height="480">

ファイルを開くとエディタモードになります。ここでファイルを編集していきます。

<img src="http://ofruby.ongaeshi.me/images/ofruby-03.png" width="320" height="480">

Runボタンを押すと実行されます。Backボタンを押すと戻ります。

<img src="http://ofruby.ongaeshi.me/images/ofruby-04.png" width="320" height="480">

[?]ボタンを押すとヘルプが表示されます。

<img src="http://ofruby.ongaeshi.me/images/ofruby-05.png" width="320" height="480">

関数名を間違えてもちゃんと教えてくれます。

<img src="http://ofruby.ongaeshi.me/images/ofruby-06.png" width="320" height="480">

## サンプルコード

### hello.rb

円、文字、動く円を書きます。

```ruby
def setup
  set_background 128, 128, 128
  @y = 0
end

def update
  @y += 1
end

def draw
  set_color 50, 200, 50
  circle 160, 200, 100

  set_color 200, 50, 50
  circle 200, @y, 50

  set_color 0, 0, 0
  text "Hello world!", 120, 160
end
```

<img src="http://ofruby.ongaeshi.me/images/ofruby-sample-hello.png" width="320" height="480">

### lines.rb

線を横に引きながら色を変えていきます。

```ruby
def setup
end

def update
end

def draw
  0.step 99 do |i|
    r = i / 100 * 255
    set_color r, 0, 0, 128
    line 0, 0, i * 20, 480
  end
end
```

<img src="http://ofruby.ongaeshi.me/images/ofruby-sample-lines.png" width="320" height="480">


## openFrameworksとRubyのメリット

openFrameworksはシンプルかつ分かりやすいAPIで簡単にグラフィックを操作することが出来ます。またC++で書かれているためmrubyを使って簡単にRubyで操作することが出来ます。

モバイルのプログラミング環境を作る際、通常のキーボードと比べてソフトウェアキーボードの入力性能の悪さはとても大きな問題です。Rubyのよい所として「必要ないことは省略出来る」が挙げられます。例えば座標(100,100)に半径10の円を書く時、Cスタイルの言語であれば

```
circle(100, 100, 10);
```

と書く必要がある所をRubyであれば

```
circle 100, 100, 10
```

と括弧やセミコロンを省略することが出来ます。キーボード環境であれば大した違いではないかもしれませんがソフトウェアキーボードになると大きな違いとなります。やってみると分かるのですがとにかく記号を打つのが面倒くさいのです。

将来的には専用ソフトウェアキーボードなどを用意して入力をサポートしていければよいな、と思っていますが素の状態でもそこそこ簡単に打てる、というのは大切なことだと思っています。

## 作った経緯
スマートフォンは世界で最も普及したコンピューターです。そこでプログラムが書けるようになればもっとたくさんの人達がプログラムを学ぶことが出来ると思い作成しました。

子供達やプログラムに興味があるけどやったことがない、PCを持っていない人達がプログラミングを始めるきっかけになればうれしいです。

・・と思って作成したのですが、作ってみたらプログラミング環境を手軽に持ち運べることがプログラマにとってもとても楽しいことに気がつかされました。電車の移動中やちょっとした待ち時間に思いつきをコードに落とし込むことが出来ます。今見ている景色をそのまま図形に変換して動かしてみたりしてます。BASICでカタカタと夢中になって作っていた頃を思い出しました。

と、というわけで大人も子供も興味がある人は是非使ってみてください。

※書いたコードをブログやTwitterに#ofrubyタグで公開してくれるとさらに嬉しいです。

## Links

ソースコードも公開していますのでプルリクエストやバグ報告も是非。

- [ongaeshi/ofruby-ios](https://github.com/ongaeshi/ofruby-ios)






