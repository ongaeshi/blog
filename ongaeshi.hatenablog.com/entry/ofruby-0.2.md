---
Title: ofruby 0.2 リリース - タッチ入力が取得出来るようになりました
Category:
- ofruby
- ruby
- mruby
- openframeworks
- C++
- ios
Date: 2014-09-16T08:39:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-0.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815732681282
---

ofruby 0.2 をリリースしました。

## 新機能
`width`, `height`でウィンドウのサイズを取得出来るようになったり、`Input.touch(idx)`, `Input.touches`でマルチタッチ入力を取得出来るようになりました。`set_background_auto`をfalseに設定すると毎Fフレームバッファをクリアせずに加算表示されるようになります。(Processingのデフォルト)

- width, height
- Input.touch(idx), Input.touches
- TouchPoint#valid?, x, y, press?, down?, release?
- set_background_auto
- set_circle_resolution
- Rename from get_frame_rate to frame_rate


ダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

## タッチ入力のサンプル

画面にタッチした押した場所にボールを落とします。RubyのArray#find_allを使うことでとても簡単にマルチタッチにも対応出来ました。


<iframe src="https://youtube.googleapis.com/v/1P3dx6WkQ5U&amp;source=uds" allowfullscreen="" frameborder="0" height="480" width="320"></iframe><br>


```ruby
def setup
  @balls = []
  @prev = false
end

def update
  ta = Input.touches.find_all { |e| e.release? }
  ta.each do |t|
    @balls.push Ball.new t.x, t.y, 0
  end
  @balls.each { |v| v.update }
end

def draw
  set_color_hex 0
  #text "#{Input.touch(0).x}", 0, 100
  @balls.each { |v| v.draw }
  set_color_hex 0x864040
  rect_rounded 0, 420, 320,20, 10
end

class Ball
  def initialize(x, y, init_speed)
    @x = x
    @y = y
    @accel = 0.098
    @speed = init_speed
    @color = rand(0xffffff)
  end

  def update
    @speed += @accel
    @y += @speed
    if @y > 410
      @y = 410
      @speed *= -0.8
    end
  end

  def draw
    #set_color_hex 0xd73a45
    set_color_hex @color
    set_fill
    circle @x, @y, 10
  end
end
```

## 0.3

AppStoreには申請済みです。次はnoise関数やMath.sinが使えるようになります。

## RubyKaigi 2014

[RubyKaigi 2014 | Lightning Talks](http://rubykaigi.org/2014/LT) で ofruby のことをしゃべります。ofrubyが動く実機(ただのiPod Touch)も持っていきますので興味のある人は@ongaeshiに声をかけて下さい。

## Links

- [ofruby開発日誌(1) - タッチ操作の組み込み - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/ofruby-touch-point)
- [iPhoneでRubyとopenFrameworksを使ってグラフィックプログラミングができるofrubyを作りました - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/ofruby-released)
- [ongaeshi/ofruby-ios](https://github.com/ongaeshi/ofruby-ios)







