---
Title: ' ofruby 0.3 をリリース - translate, rotate, scale が使えるようになりました'
Category:
- openframeworks
- ruby
- mruby
Date: 2014-09-24T23:30:11+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-0.3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815733558062
---

ofruby 0.3 をリリースしました

## 新機能
実行画面のナビゲーションバーを隠すことで全画面が使えるようになりました(結構大切) 。`noise`, `Math::sin`, `deg_to_rad`, `push_matrix`, `translate`, `rotate`, `scale` などが使えるようになりました。

- Hide the navigationBar at executing script
- noise(x), noise(x, y), noise(x, y, z)
- Ruby's Math module (sin, cos ..)
- rad_to_deg, deg_to_rad
- push_matrix, pop_matrix, translate, rotate
- dist, dist_squared, clamp, lerp
- Support a hardware keyboard

ダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

## 電光掲示板
push_matrix, translate, rotate, scale で文字や図形を加工することが出来るようになったので電光掲示板を作ってみました。文字を変更するとiPhoneが自作のスクリーンセーバーになります。

push_matrixが何をしているのかというのは[この辺り](http://www.myu.ac.jp/~xkozima/lab/ofTutorial2.html)が詳しいです(ofPushMatrixで検索)。これ以外にも基本的にopenFramworksやProcessingの概念はそのままofrubyにも使えるはずです。後Rubyのおかげでブロックを利用すれば<b>pop_matrixの呼び出しは不要</b>です。

![touch_fall.png](https://raw.githubusercontent.com/ongaeshi/ofruby-sample/master/images/text_board.png)

[ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#08-text-board)

```ruby
TEXT = "Hello, ofruby!"
# TEXT = "abcdefghijklmnopqrstuvwxyzANCDEFGHIJKLMNOPQRSTIVWXYZ1234567890-/:;()&@.,?!'"
TEXT_WIDTH = 8
SCALE = 3
SPEED = 3
COLOR = rand 0xffffff
BACK = 0x000000

def setup
  set_background_hex BACK
  @texts = []
  @texts.push Texter.new(TEXT)
end

def update
  @texts.each { |e| e.update }
end

def draw
    @texts.each { |e| e.draw }
end

class Texter
  def initialize(text)
    @text = text
    @width = text.length * TEXT_WIDTH * SCALE
    @x = 0
    @y = height / 2
    @rot = 0
  end

  def update
    @x += SPEED
    @x = -@width if @x > width + @width
  end

  def draw
    push_matrix do
      translate @x, @y
      scale SCALE, SCALE * 2
      set_color_hex COLOR
      text @text, 0, 0
    end
  end
end
```

## 0.4

[RubyKaigi](http://ongaeshi.hatenablog.com/entry/rubykaigi-2014)での要望を入れていきたいと思います。今はスクリプトの削除機能を作っています。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140924/20140924232950.png" alt="f:id:tuto0621:20140924232950p:plain" title="f:id:tuto0621:20140924232950p:plain" class="hatena-fotolife" itemprop="image"></span></p>


## Links

- [ofruby 0.2 リリース - タッチ入力が取得出来るようになりました](http://ongaeshi.hatenablog.com/entry/ofruby-0.2)
- [ofruby開発日誌(1) - タッチ操作の組み込み - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/ofruby-touch-point)
- [iPhoneでRubyとopenFrameworksを使ってグラフィックプログラミングができるofrubyを作りました - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/ofruby-released)
- [ongaeshi/ofruby-ios](https://github.com/ongaeshi/ofruby-ios)

