---
Title: ' ofrubyのコード集(1) - ストライプを表示する'
Category:
- ofruby
- openframeworks
- ruby
- mruby
- ios
Date: 2014-08-29T00:41:42+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-sample-stripe
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815731532572
---

ofrubyのダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>


[p5.js](http://p5js.org/learn/examples/Structure_Width_and_Height.php)の真似です。`step`を使って0〜480まで20刻みで線を書いていきます。`set_color_hex`を使うと色を16進数で指定することが出来ます。#ff00ffのようなWebの色指定と同じなので、Kulerなどのカラー調整アプリが使いやすくなります。

```ruby
WIDTH = 320
HEIGHT = 480
SIZE = 20

def draw
  set_background_hex 0xffffff
  set_fill

  0.step HEIGHT, SIZE*2 do |i|
    set_color_hex 0xC9F8F1
    rect 0, i, WIDTH, SIZE
    set_color_hex 0xCA3C6E
    rect i, 0, SIZE, HEIGHT
  end
end
```

![stripe.png](https://raw.githubusercontent.com/ongaeshi/ofruby-sample/master/images/stripe.png)

[ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#striperb)






