---
Title: PictRubyでGUIプログラミングの参考に Ruby Shoes を動かす
Category:
- diary
- rubypico
Date: 2016-03-08T23:26:45+09:00
URL: https://ongaeshi.hatenablog.com/entry/install-ruby-shoes
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792366294577
---

PictRubyのGUIはこんな感じに書きたいと考えている。

```ruby
app do
  button "Tap Me" do
    Popup.msg "Hello!"
  end
end
```

上のプログラムを実行すると"Tap Me"というボタンが表示され、タップすると"Hello!"というメッセージがポップアップする。
appのブロック内にGUIの初期化処理を書く。コードを読むとなんとなく何が起こるか分かるのではないだろうか？

上のようなコードをなんとなく見たことあるなら、それは[Ruby Shoes](http://shoesrb.com/)です。Ruby Shoesのスーパーシンプルな記述方法は(入力環境が貧弱なために)1文字でも冗長なコードを書きたくないモバイルプログラミングにぴったりだと感じている。

良い機会なので実際に Ruby Shoes をインストールして動かしたり、コードを読んだりしてPictRubyの参考にしたいのだが、悲しいことにOSXでは最新の El Capitan で動かない・・。仕方がないので Parallel Desktop 上の Windows 7 で動かすことにする。

[Downloads](http://shoesrb.com/downloads/)から"Shoes 3.3.0 for Windows 7+ 32 bit"をダウンロード。ダブルクリックすると普通にインストーラが起動する。fc-cache.exeの実行に結構に時間がかかるがインストールが終了。

[Tutorial](http://shoesrb.com/walkthrough/)を上から順に実行していく。スクリプトを実行するにはアイコンに.rbをドラッグ＆ドロップすれば良い。

[f:id:tuto0621:20160308232524j:plain]

以下は上から順にチュートリアルを実行した時の履歴。

```ruby
# Shoes.app {
#   image "http://spiralofhope.com/i/ruby-shoes--nks-kidnap.png"
# }

# Shoes.app {
#   para strong("Q."), " Are you beginning to grasp hold of Shoes?"
# }

# Shoes.app {
#   stack(margin: 6) {
#     title "A Question"
#     para strong("Q."), " Are you beginning to grasp hold of Shoes?"
#     para em(strong("A."), " Quit pestering me, I'm hacking here.")
#   }
# }

# Shoes.app {
#   @push = button "Push me"
#   @note = para "Nothing pushed so far"
# }

# Shoes.app {
#   @push = button "Push me"
#   @note = para "Nothing pushed so far"
#   @push.click {
#     @note.replace "Aha! Click!"
#   }
# }

# Shoes.app do
#   background "#F3F".."#F90"
#   title("Shoooes",
#         top:    60,
#         align:  "center",
#         font:   "Trebuchet MS",
#         stroke: white)
# end

# Shoes.app do
#   background "#EFC"
#   border("#BE8",
#          strokewidth: 6)

#   stack(margin: 12) do
#     para "Enter your name"
#     flow do
#       edit_line
#       button "OK"
#     end
#   end
# end

# Shoes.app do
#   @shape = star(points: 5)
#   motion do |left, top|
#     @shape.move left, top
#   end
# end

# Shoes.app do
#   @shoes = image(
#     "http://spiralofhope.com/i/ruby-shoes--shoes.png",
#     top:  100,
#     left: 100
#   )
#   animate do |i|
#     @shoes.top += (-20..20).rand
#     @shoes.left += (-20..20).rand
#   end
# end

Shoes.app do
  @poem = stack do
    para "My eyes have blinked again
    And I have just realized
    This upright world
    I have been in.
    My eyelids wipe
    My eyes hundreds of times
    Reseting and renovating
    The scenery."
  end
  para(
    link("Clear").click do      # linkコマンドでリンク貼れるのいいなぁ
      @poem.clear
    end
  )
end

# Shoes.app(width: 300, height: 400) do
#   fill rgb(0, 0.6, 0.9, 0.1)
#   stroke rgb(0, 0.6, 0.9)
#   strokewidth 0.25

#   100.times do
#     oval(left:   (-5..self.width).rand,
#          top:    (-5..self.height).rand,
#          radius: (25..50).rand)
#   end

#   button("Regenerate") do
#     100.times do
#       oval(left:   (-5..self.width).rand,
#            top:    (-5..self.height).rand,
#            radius: (25..50).rand)
#     end
#   end
# end
```

一通り動かすことができた。次は[Manual](http://shoesrb.com/manual/Hello.html)を読んだり、他のアプリを動かしたりしてみよう。

