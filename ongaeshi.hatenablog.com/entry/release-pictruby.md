---
Title: iPhoneでRubyを使って写真フィルターが書けるPictRubyをリリースしました
Category:
- rubypico
- ruby
- mruby
- picture
- ios
Date: 2015-11-08T08:25:47+09:00
URL: https://ongaeshi.hatenablog.com/entry/release-pictruby
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415126156556
---

[PictRuby - Photo editor that can write a filter in Ruby](http://pictruby.ongaeshi.me/)

iOS上でコードを編集、実行、デバッグしながら写真を加工するフィルターをRubyで書くことができます。複数の写真をまとめて変換することも可能です。

![editor](http://pictruby.ongaeshi.me/images/pictruby-screen-all.jpg)

## Hello, world

PictRubyのルールはたった1つです

- `convert`という名前の関数を作って画像(`class Image`)を返す

まずは画像を選択してそのまま表示してみます。

```ruby
def convert
  #Image.load("sample.jpg")
  Image.pick_from_library
end
```

`Image.pick_from_library`の仕事は画像選択ダイアログを表示→OK→選択された画像を`Image`に変換して返します。以下のような結果が返ってきます。

![none](http://pictruby.ongaeshi.me/images/none.jpg)

※ pick_from_libraryの代わりに`Image.load`を使うと↑のサンプル画像を表示することができます

## Hello, (gray) world

画像をグレイスケールに変換してみましょう。Imageクラスには組み込みでたくさんの便利なメソッドが用意されています。(エディタの右上にある`[?]`を押すとメソッド一覧)

`Image#gray`を呼ぶとグレイスケールに変換した画像を新たに作って返します。(呼び出し元の画像は書き換えません) 

1. 画像を選択
1. ローカル変数`img`に受ける
1. `img.gray`でグレイスケールに変換した画像を返す

```ruby
def convert
  img = Image.pick_from_library  
  img.gray
end
```

![gray](http://pictruby.ongaeshi.me/images/gray.jpg)

`gray`を`Image#sepia`や`Image#invert`に変えて遊んでみましょう。

![sepia](http://pictruby.ongaeshi.me/images/sepia.jpg)
![invert](http://pictruby.ongaeshi.me/images/invert.jpg)

## グリッドに並べる

複数枚の写真を引数に受け取って一枚の画像を作ることもできます。グリッドに並べてみましょう。

```ruby
def convert
  imgs = Image.pick_from_library(81)
  ImageUtil.grid(imgs)
end
```

`Image.pick_from_library`の第1引数に最大枚数を渡すと結果が配列で返します。`ImageUtil.grid`に渡すとグリッド状に並べた画像を返してくれます。仲間に`ImageUtil.vertical`, `ImageUtil.horizontal` がありこの3つの関数を組み合わせることで好きな形にレイアウトすることができます。

![grid](http://pictruby.ongaeshi.me/images/grid.jpg)


## 正方形に切り取ってグリッドに並べる

次は正方形に切り取ってからグリッドに並べます。gridとgrid_squareの違いは`ImageUtil.grid`に渡す前に正方形に変換するかどうかです。Rubyらしくなってきましたね。

```ruby
def convert
  imgs = Image.pick_from_library(81)
  ImageUtil.grid(imgs.map { |e| e.square })
end
```

![grid_square](http://pictruby.ongaeshi.me/images/grid_square.jpg)

ちなみに`Image#square`は組込み関数ですがRubyで書かれています。以下のようなコードで正方形に切りとることができます、シンプルですね。(そしてオープンクラス便利！)

```ruby
class Image
  def square(x_offset = 0, y_offset = 0)
    l = width > height ? height : width
    cx = width / 2
    cy = height / 2
    crop(cx - l/2 + x_offset, cy - l/2 + y_offset, l, l)
  end
end
```

## Image#render

実は`ImageUtil#grid`もRubyで書かれています。

[PictRuby/builtin.rb](https://github.com/ongaeshi/PictRuby/blob/master/resources/builtin.rb)

```ruby
class ImageUtil
  def self.grid(imgs)
    grid = grid_size(imgs.length)

    width = imgs[0].width
    height = imgs[0].height

    ygrid = (imgs.length / grid).to_i 
    ygrid += 1 if imgs.length % grid > 0

    Image.render(width*grid, height*ygrid) do 
      (0...grid).each do |y|
        (0...grid).each do |x|
          img = imgs[y*grid + x]
          img.draw(x*width, y*height, width, height) if img
        end
      end
    end
  end
end
```

`Image#render`が肝で引数にサイズを渡してブロック内で`Image#draw`を呼ぶことで任意の画像を作ることができます。

## サンプルコード

ここで紹介したコードは全てサンプルタブに含まれているので、プログラムを書かなくても、フィルターにかけたり、グリッドに並べたり、画像をリサイズしたり、それらを組み合わせて目的の画像を作ることができます。

PictRubyで現在何ができるのかは[こちら](http://pictruby.ongaeshi.me/)を参考にしてください。

それではモバイルコンピュータでもプログラミングを楽しみましょう。





