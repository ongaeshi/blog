---
Title: mrubyでインタラクティブアプリケーションを作ろう！RubyKokubanで画像が表示出来るようになりました
Category:
- rubykokuban
- ruby
- mruby
- openframeworks
- game
- osx
Date: 2013-10-21T12:20:26+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubykokuban-image
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815711145563
---

前回リリースからさらに機能を追加しました。RubyKokubanの基本的な使い方やインストール方法などは[こちら](http://ongaeshi.hatenablog.com/entry/rubykokuban-release)をどうぞ。

- 画像の表示 (Image)
- 色を制御するクラスを追加 (Color)

## 画像を表示する
`Image.load(filename)`を使います。適当な画像をスクリプトと同じ場所に置いて下さい。1行目の`||=`は最初の一回だけ初期化コードを実行するRubyでよくあるイディオムです。

```ruby
def draw
  @image ||= Image.load("sample.png")
  set_color(255, 255, 255)
  @image.draw(0, 0)
end
```

実行します。

```
$ kokuban exec image001.rb
```

左上の(0. 0)に画像が表示されました。簡単ですね。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131020/20131020173940.png" alt="f:id:tuto0621:20131020173940p:plain" title="f:id:tuto0621:20131020173940p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## set_colorはなんのため？
Image#drawの際にset_colorで設定された値と乗算された色で描画されます。

```ruby
def draw
  @image ||= Image.load("sample.png")
  set_color(0, 255, 255)
  @image.draw(0, 0)
end
```

例えばset_colorをR=0にすると下記のようにR成分が0になった状態で描画されます。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131020/20131020174011.png" alt="f:id:tuto0621:20131020174011p:plain" title="f:id:tuto0621:20131020174011p:plain" class="hatena-fotolife" itemprop="image"></span></p>

**画像そのままの色** で出力したい時は `set_color(255, 255, 255)` もしくは `set_color(Color::White)` を設定して下さい。

## 拡大縮小させる
次はアニメーションさせてみましょう。

```ruby
def update
  @scale ||= 0.0
  @scale += 0.05
  @scale = 0.0 if @scale > 3.0
end

def draw
  @image ||= Image.load("sample.png")
  set_color(Color::White)
  @image.draw(0, 0, @image.width * @scale, @image.height * @scale)
end
```

update関数で`@scale`メンバを増加させてImage.drawにパラメータとして渡しています。

```
$ kokuban exec image003.rb
```

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131021/20131021234835.gif" alt="f:id:tuto0621:20131021234835g:plain" title="f:id:tuto0621:20131021234835g:plain" class="hatena-fotolife" itemprop="image"></span></p>

## マウスに追従させる
最後はマウスで動かせるようにしてみます。

```ruby
def setup
  @image = Image.load("sample.png")
  @image.set_anchor_percent(0.5, 0.5)
end

def draw
  set_color(Color::White)
  @image.draw(Input.mouse_x, Input.mouse_y)
end
```

```
$ kokuban exec image004.rb
```

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131020/20131020174044.gif" alt="f:id:tuto0621:20131020174044g:plain" title="f:id:tuto0621:20131020174044g:plain" class="hatena-fotolife" itemprop="image"></span></p>

## さらに詳しく
- Imageの詳しい使い方
  - [ongaeshi/rubykokuban-sample/image.rb](https://github.com/ongaeshi/rubykokuban-sample#imagerb)
- Image#each_pixelsを使った画像処理
  - [ongaeshi/rubykokuban-sample/image_filter.rb](https://github.com/ongaeshi/rubykokuban-sample#image_filterrb)
- シューティングゲーム
  - [ongaeshi/rubykokuban-sample/mouse_shooting2.rb](https://github.com/ongaeshi/rubykokuban-sample#mouse_shooting2rb)
- Qiitaに投稿したやつ
  - [Ruby - 画像をリアルタイムにグレースケールにしていくプログラム - Qiita [キータ]](http://qiita.com/ongaeshi/items/5eeeb0888b1ca853dec0)


RubyKokuban0.2で簡単なゲームみたいなものなら作れるようになったのではないかと思います。
何か作った人がいましたら是非GitHubやTwitterなどで公開してもらえたら励みになります！

