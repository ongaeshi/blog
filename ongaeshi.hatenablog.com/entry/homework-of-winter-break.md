---
Title: 冬休みの宿題にEnglish Grammer in Useをやる
Category:
- english
Date: 2014-12-22T23:37:06+09:00
URL: https://ongaeshi.hatenablog.com/entry/homework-of-winter-break
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450077778182
---

年末年始を使って英語の勉強をやることにした。RubyKaigiで英語のスピーチがほとんど分からなかったのでなんかやる気になった。

教科書はEnglish Grammer in Use。

[asin:052118939X:detail]

問題集として以下のアプリも購入。

[Murphy's English Grammar in Use](https://itunes.apple.com/jp/app/murphys-english-grammar-in/id848215354?mt=8)

## やり方
- Unit 1 から順番にやる
- 教科書の左側のページを読む、読みながらiPadのメモを使ってメモをとる
- 問題集アプリを起動して読んだページの問題を解く、2つあるけど両方やると時間がかかるので最初の10問だけ

教科書は図入りで分かりやすく文法を紹介してくれるのでかなりよい。頑張ってみます。

## やる気を持続させるために
[ofruby](http://ofruby.ongaeshi.me/)を使って残りのUnit数を表示するアプリを作った。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141222/20141222234733.png" alt="f:id:tuto0621:20141222234733p:plain" title="f:id:tuto0621:20141222234733p:plain" class="hatena-fotolife" itemprop="image"></span></p>

`@out`にその日にやったUnit数を記録していくだけ。そのうちグラフ対応もする。

```ruby
TEXT_WIDTH = 8

def setup
  @out = [
    6, 5, 0, 0, 1, 0, 0, 0, 4
  ] # 12/21
  @num = @out.reduce(141) { |a, e| a - e }
end

def update
end

def draw
  set_color 255, 0, 0 if @num < 0
  translate width / 2, height / 2
  scale 5, 5

  offset_x = -@num.to_s.length * TEXT_WIDTH * 0.5
  text "#{@num}", offset_x, 0

  set_color 0, 0, 0
  text "day #{@out.size}", -18, -13
end
```
