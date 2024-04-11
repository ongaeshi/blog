---
Title: ' ofrubyのコード集(2) - 基本図形と文字の描画'
Category:
- ruby
- openframeworks
- mruby
- ofruby
Date: 2014-08-29T23:45:43+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-sample-hello
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815731585801
---

ofrubyのダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

動かない円、動く円、文字を表示します。後に書いたものが描画が優先されます。他の基本図形の書き方を知りたい時はエディタ画面の[?]を見て下さい。

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

![hello.png](https://raw.githubusercontent.com/ongaeshi/ofruby-sample/master/images/hello.png)

[ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#hellorb)






