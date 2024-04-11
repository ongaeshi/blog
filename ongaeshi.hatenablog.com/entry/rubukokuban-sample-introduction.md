---
Title: RubyKokubanのサンプルコードを紹介
Category:
- ruby
- rubykokuban
- openframeworks
- osx
Date: 2013-09-25T12:30:06+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubukokuban-sample-introduction
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318758074813
---

[前回の紹介](http://ongaeshi.hatenablog.com/entry/rubykokuban-release)が思った以上に多くの人に反応してもらえて嬉しい限りです。早く画像を貼付けたり(今やってます)音を鳴らしたい所です。

RubyKokubanで何が出来るのかよく分からない人が大半だと思うので今日はサンプルコードの紹介をしようと思います。全てのサンプルコードは[ongaeshi/rubykokuban-sample](https://github.com/ongaeshi/rubykokuban-sample)にあります。

## 基本図形の描画(shapes.rb)
各図形を任意の位置に描画するシンプルなサンプルです。

- 色指定
- (オプション)線の太さを指定
- (オプション)塗りを指定
- 位置を指定して描画

しています。draw関数がメインでsetup関数はウィンドウのサイズを調整しているだけです。

![rubykokuban-sample/shapes.rb](https://raw.github.com/ongaeshi/rubykokuban-sample/master/images/shapes.png)

```ruby
def setup
  set_window_size(580, 600)
  # set_window_pos(0, 0)
end

def draw
  set_background(255, 255, 255)
  
  set_fill

  set_color(87, 25, 122)
  text("circle", 125, 90)
  circle(150, 150, 50)

  set_color(220, 73, 0)
  text("triangle", 370, 90)
  triangle(350, 200, 450, 200, 400, 114)

  set_no_fill
  
  set_color(51, 106, 21)
  text("ellipse", 120, 240)
  set_line_width(5)
  ellipse(150, 300, 150, 100)

  set_color(0, 0, 0)
  text("line", 380, 240)
  set_line_width(10)
  line(350, 250, 450, 350)
  line(450, 250, 350, 350)

  set_fill

  set_color(196, 0, 0)
  text("rect", 135, 390)
  rect(100, 400, 100, 100)

  set_line_width(1)
  set_color(196, 0, 230)
  text("rect_rounded", 350, 390)
  rect_rounded(350, 400, 100, 100, 30)

  set_color(0, 0, 0)
  
  text(DebugInfo.fps, 10, 15)
  text(DebugInfo.window, 10, 30)
  text(DebugInfo.mouse, 10, 45)
end
```

## 図形を動かす(move_circles.rb)
マウス入力を受け取り、図形を動かすサンプルです。

![rubykokuban-sample/move_circles.rb](https://raw.github.com/ongaeshi/rubykokuban-sample/master/images/move_circles.png)

openFrameworksだとマウス入力は専用のコールバック関数(mousePressed()等)を経由して取得するのですが、それだと入力判定と実際のアップデート処理が

- 毎Fの座標更新はupdate()
- 速度の増減はmousePressed()

とコード的に離れてしまうので **Input** クラスを専用に用意しました。

Inputクラスを使うと左クリックでスピードアップ、右ボタンを押しっぱなしでブレーキ、みたいな処理をupdate関数内で完結させることが出来ます。

RubyKokubanにもmouse_pressedコールバックは用意されていますが、openFrameworksを使う時に比べて使う機会は少ないです。<span style="color: #cc0000"><b>シンプルに setup, update, draw さえ書けば</b></span>インタラクティブなアプリケーションが書ける事を目指しています。

```ruby
def setup
  @x = 400
  @speed = 10
end

def update
  @speed += 5 if Input.mouse_press?(0)
  @speed -= 5 if Input.mouse_down?(2) 

  @x += @speed

  if @x > 1024
    @x = 0
  elsif @x < 0
    @x = 1024 
  end
end

def draw
  set_color(255, 0, 0)
  circle(@x, 100, 30)

  set_color(0, 255, 0)
  circle(@x + 20, 200, 35)

  set_color(0, 0, 255)
  circle(@x + 40, 300, 40)

  c = (@x * 0.7) % 255
  set_color(c, c, c)
  circle(@x + 60, 400, 45)

  set_color(0, 128, 128)
  circle(Input.mouse_x, Input.mouse_y, 30)

  set_color(0, 0, 0)
  text(DebugInfo.fps, 10, 15)
  text(DebugInfo.window, 10, 30)
  text(DebugInfo.mouse, 10, 45)
  text(<<EOF, 10, 60)
speed       : #{@speed}
mouse left  : speed up
mouse right : speed down
EOF
end
```
