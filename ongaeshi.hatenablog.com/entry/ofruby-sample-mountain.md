---
Title: ' ofrubyのコード集(3) - マウンテン'
Category:
- ruby
- mruby
- openframeworks
- ofruby
Date: 2014-09-01T23:13:57+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-sample-mountain
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815731734594
---

ofrubyのダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

山に行った時に作りました。一定時間が経つと夜になって星が出てきます。

```ruby
def setup
  set_background 239, 90, 41
  @frame = 0
  @x = 0
  @y = -100
  #@night = true
end

def update
  @frame += 1
  @night = true if @frame > 950
  @x += 0.6
  @y  += 0.6
  set_background 0, 0, 0 if @night
end

def draw
  if @night
    set_color 255, 255, 255
    star 100, 100
    star 49, 24
    star 123, 42
    star 300, 200
    star 50, 150
    star 300, 50
  else
    c 100, @y
  end
  set_color 45, 150, 50
  m 0, 480, 150, 70
  m 100, 480, 200, 150
  m 200, 480, 200, 50
  # set_color 0, 0, 0
  # text "#{@frame}", 0, 100
end

def m(x, y, w, h, rate = 0.5)
  triangle x, y, x + w, y, x + w*rate, y - h
end

def c(x, y)
  set_color 240, 30, 30
  circle x, y, 50
end

def star x, y
  circle x, y, 1
  circle x+100, y+55, 1
  circle x+80, y+10, 1
end
```

![mountain.gif](https://raw.github.com/ongaeshi/ofruby-sample/master/images/mountain.gif)

[ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#mountain)






