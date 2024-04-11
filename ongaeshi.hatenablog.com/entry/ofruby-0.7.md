---
Title: 絵文字を表示して自由に回転やスケーリングをかけられるようになりました - ofruby 0.7
Category:
- ofruby
- ruby
- openframeworks
- ios
Date: 2015-05-06T23:23:02+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-0.7
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450093620597
---

ofruby 0.7 をリリースしました。
ofrubyはiPhoneやiPad上でRuby+openFrameworksを使って簡単にグラフィックプログラムを書くことが出来るアプリです。

[f:id:tuto0621:20150506231611g:plain]

0.7では画像を表示するための機能を組み込みました(Imageクラス)。将来的には任意の画像を追加、表示出来るようにする予定ですがそのためには画像ファイルを組み込むためのインターフェースを一通り用意する必要があります。

ユーザーの人からみても最終的にはオリジナル画像に置き換えるとしても、とりあえず仮で表示するための組み込み画像がたくさん用意されていると便利です。

そこで組み込みの画像として絵文字を表示出来るようにしました。通常の絵文字と違って移動(translate)、回転(rotate)、拡大(scale)をかけたり、タッチ操作と連動して任意の位置に動かすことが可能です。実質的には組み込みのスプライト画像として使えます。

[f:id:tuto0621:20150506231633p:plain]

## 新機能
- 絵文字の表示
- mruby 1.1 に更新(Rubyの基本性能アップ)
- サンプル追加
  - emoji.rb (使える絵文字の一覧表示)
  - emoji_quiz.rb (絵文字を覚えるためのクイズゲーム)

ダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

## 絵文字の呼び出し方
`Image.emoji(name)`で絵文字を画像データとして表示することが出来ます。
通常は左上が基準座標となりますが`set_anchor_percent(x, y)`を使うことで中心や足下に基準を変更したり、`mirror(vertical, horizontal)`で画像を上下左右反転させられます。

```ruby
# emoji_view.rb

def setup
  @face = Image.emoji(:grimacing)
  @face.set_anchor_percent(0.5, 0.5)

  @tengu = Image.emoji(:japanese_goblin)
  @tengu.set_anchor_percent(0.5, 0.5)

  @snowman = Image.emoji(:elephant)
  @snowman.set_anchor_percent(0.5, 0.5)
  @snowman.mirror(false, true)
end

def draw
  @face.draw(width / 2, 100)

  push_matrix do
    translate width/2, 200
    rotate 45
    @tengu.draw(0, 0)
  end

  push_matrix do
    translate width/2, 350
    rotate -20
    scale 3, 3
    @snowman.draw(0, 0)
  end  
end
```

## サンプル: emoji.rb
使える絵文字を調べたり呼び出すためのシンボル名を調べるのに便利です。

[emoji.rb](https://github.com/ongaeshi/ofruby-sample/blob/master/emoji.rb)

[f:id:tuto0621:20150506004355p:plain]

## サンプル: emoji_quiz.rb
絵文字クイズです。シンボル名から正しい絵文字を選んでいきましょう。

[emoji_quiz.rb](https://github.com/ongaeshi/ofruby-sample/blob/master/emoji_quiz.rb)

[f:id:tuto0621:20150506004406p:plain]

## 冒頭のサンプルコード
回したり動かしたりスケールをかけるのは以下のような感じで書きます。

[f:id:tuto0621:20150506232344p:plain]

```ruby
# sprite.rb

CX = width / 2
CY = height / 2
GY = height - 50

def setup
  set_background_hex 0xcbe0f5

  @pig = Sprite.new(CX + 50, CY, Image.emoji(:pig2))
  @pig.set_anchor_center
  @pig.sx = @pig.sy = 3

  @bicyclist = Sprite.new(50, GY, Image.emoji(:bicyclist))
  @bicyclist.set_anchor_bottom
  @bicyclist.mirror(false, true)
  @bicyclist.vx = 2

  @palm_tree = Image.emoji(:palm_tree)
  @palm_tree.set_anchor_percent(0.5, 1.0)
  @palm_tree.resize(@palm_tree.width, 70)
end

def update
  @pig.rotate -= 5
  @bicyclist.update
  @bicyclist.x = 0 if @bicyclist.x > width
end

def draw
  set_color_hex 0x7ace32
  rect 0, GY, width, height

  @pig.draw

  set_color_hex 0xffffff
  0.step(width, 40) do |x|
    @palm_tree.draw(x, GY)
  end

  @bicyclist.draw
end

#---------------------------------------------------------------

class Sprite
  attr_accessor :x, :y, :rotate, :sx, :sy
  attr_accessor :vx, :vy

  def initialize(x, y, image)
    @x = x
    @y = y
    @image = image
    @rotate = 0
    @sx = 1
    @sy = 1
    @vx = @vy = 0
  end

  def mirror(x, y)
    @image.mirror(x, y)
  end

  def set_anchor_center
    @image.set_anchor_percent(0.5, 0.5)
  end

  def set_anchor_bottom
    @image.set_anchor_percent(0.5, 1.0)
  end

  def update
    @x += @vx
    @y += @vy
  end

  def draw
    push_matrix do
      translate @x, @y
      Kernel.rotate @rotate
      scale @sx, @sy

      set_color_hex 0xffffff
      @image.draw(0, 0)
    end
  end
end
```

絵文字だけでも結構色々なゲームが作れるんじゃないかという気がしてきました。皆さんも何か作ったら是非Twitterやブログに投稿してください。

## Links
- [iPhoneで実際に動くサンプルコードを見ながらRubyを使ってゲームを作れるように - ofruby 0.6](http://ongaeshi.hatenablog.com/entry/ofruby-0.6)
- [iPhoneでRubyを書いてマッチ3パズル作った](http://ongaeshi.hatenablog.com/entry/ofruby-match3)
- [ofruby 0.3 をリリース - translate, rotate, scale が使えるようになりました](http://ongaeshi.hatenablog.com/entry/ofruby-0.3)
