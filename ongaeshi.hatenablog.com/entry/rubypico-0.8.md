---
Title: RubyPico 0.8 リリース - mainが不要に、画像がputs可能に
Category:
- rubypico
Date: 2016-08-16T11:41:18+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.8
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687179311836
---

RubyPico 0.8 をリリースしました。以前に[紹介](http://ongaeshi.hatenablog.com/entry/rubypico-0.6)したようにメインルーチンをmain関数で囲まずに実行できるようになり、よりRubyらしく書けるようになりました。

```ruby
puts "Hello, RubyPico!"
puts "http://rubypico.ongaeshi.me"
puts Image.load("chat_ruby.png")
```

[f:id:tuto0621:20160816111155p:plain]

アプリ内のサンプルコードは全て新しい書き方に書き直されているので詳しくはそちらを参考にしてください。

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## mainが不要に
CRubyと同じように書けるようになったため、Webや書籍のサンプルコードをコピペで動かすことができるようになりました。例えばこれは[初めてのRuby](http://ongaeshi.hatenablog.com/entry/first-ruby)内の素数を求めるサンプルコードですが、そのままRubyPicoで動かすことができるようになります。

```ruby
(2..100).each do |candidate|
  sqrt = Math.sqrt(candidate)
  factor_found = (2..sqrt).any? {|i| candidate % i == 0}
  if factor_found then
    print "#{candidate} は合成数\n"
  else
    print "#{candidate} は素数\n"
  end
end
```

[f:id:tuto0621:20160815214622p:plain]

## 旧コードの動かし方
main関数を明示的に呼び出すようにしてください。

```ruby
def main
  .
  .
  .
end

# プログラムの最後にmain呼び出しのコードを追加
main
```

チャットの方は動かなくなってしまいます、すいません。手元にもたくさんのチャットコードがあり悩んだのですがCRubyと同じ感覚で使えることを優先することにしました。

なお、チャットUIのようにプログラムに対して画面をオーバーレイせず入力を渡す仕組みは別途用意する予定です。

## 画像をURL指定で表示できるように
`Image.pick_from_library`でフォトライブラリから、`Image.load(path)`で[サンプル画像](https://github.com/ongaeshi/RubyPico/tree/master/resources/images)を表示することができましたが、加えてURLを指定して画像を表示することができるようになりました。

```ruby
URL = "http://rubypico.ongaeshi.me/images/rubypico_icon.png"
logo = Image.load(URL)
puts logo
```

## 複数の画像の出力が可能に
最初の実行結果でお気付きの方もいるかもしれませんが、**テキストと画像を混在して出力**させることができるようになりました。さらに**複数枚の画像を出力**することもできます。サンプルのimage.rbは選択した画像の右下にRubyPicoのロゴ画像を重ねて表示させています。

```ruby
# # image
#
# ## Description
# Display image and overray with logo

puts "image"
image = Image.pick_from_library
# image = Image.load("sample.jpg")
puts image

puts "logo"
URL = "http://rubypico.ongaeshi.me/images/rubypico_icon.png"
logo = Image.load(URL)
puts logo

puts "overray"
overrayed_image = Image.render(image.w, image.h) do
  image.draw(0, 0, image.w, image.h)

  w = image.w / 8
  h = logo.fith(w)

  img = logo.sepia
  img.draw(image.w - w,
           image.h - h,
           w,
           h)
end
puts overrayed_image
```

画像をコピー、カメラロールに保存したい場合はその画像を長押ししてください。

[f:id:tuto0621:20160816111239p:plain]
