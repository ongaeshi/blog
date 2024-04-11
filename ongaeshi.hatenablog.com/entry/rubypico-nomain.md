---
Title: RubyPico近況 - main不要で書けるようにしたい
Category:
- rubypico
Date: 2016-07-04T23:08:05+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-nomain
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653812171403970676
---

よりRubyらしく書けるように色々と手を加えています。

- (済) main不要に
  - 人に見せると一番突っ込まれるのがここ
  - やっぱり分かりにくい
  - 直接トップレベルを実行できるようにする
- (済) mrubyをサブスレッドで実行する
  - 今までYieldを使っていたような処理を全てやめる
  - 代わりにUIとのやりとりはメインスレッド(UI制御)に dispatch_sync or dispatch_async して行うことにする
- (済) 無限ループの場合もUIは止まらないように
  - サブスレッドで動かすことで実現できた
  - これで[mruby-simplehttpserver](https://github.com/matsumoto-r/mruby-simplehttpserver)のようにずっと待ち受けるようなライブラリも動くようになる
- 画像の表示
  - pやputsでテキストと一緒に表示するようにしたい
  - これで関数の戻り値で返す必要がなくなる
- チャットビューコントローラのようなUIの作り方
  - ナビゲーションコントローラにmrubyのオブジェクトを渡すことで実現したい(Rackがcallに反応するものならなんでもいいような感じにしたい)
  - mrubyはスクリプト実行中は常に1つだけ動く
  - ビューコントローラは対応するmrubyオブジェクトを自身の寿命中ずっと保持しておく(GCで回収されないように)
  - このへんはまだ構想止まり

## 進捗
こんな感じに書けるようになります。def main不要だとやっぱりすっきりしますね。

```ruby
p "hi"
p 1 + 2 + 3 + 4

(1..3).each do
  p Time.now
end

# loop do
#   p Time.now
# end

# 1 + nil

p 1 + 2 + 3

(1..10).each do |e|
  p e
end

nil + "A"
```

実行結果です。

エラー表示も実行結果と一緒に表示されるようになってよりわかりやすくなりました。無限ループしてもメッセージがつらつらと表示されるだけで<b>Backボタンを押せばいつでもmrubyの実行を停止</b>することができます。

[f:id:tuto0621:20160704230108p:plain]

リリースにはもう少し時間がかかりそうですが大分使いやすくなると思いますのでご期待ください。

最新は[feature/remove-main](https://github.com/ongaeshi/RubyPico/tree/feature/remove-main)ブランチで開発しているのでビルドすれば手元で動かすことも可能です。


