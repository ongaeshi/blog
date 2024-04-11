---
Title: iPhoneでRubyを書いてマッチ3パズル作った
Category:
- openframeworks
- ruby
- mruby
- iphone
- ios
- c++
Date: 2015-02-17T00:39:18+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-match3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450083962122
---

[f:id:tuto0621:20150217001309g:plain]

作っているiPhone+Rubyのプログラミング環境[ofruby](http://ofruby.ongaeshi.me/)に実際に遊べるゲームサンプルを追加してみました。基本図形が書けて更新処理と描画処理を分けられればシンプルなゲームは作れるはずなのでよくあるマッチ3パズルを作ってみました。gifを高精細にした動画は[こちら](https://www.youtube.com/watch?v=M6Sux2MsOiI)です。

- [ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#09-match3-game)

ソースコードは全部で[500行位](https://github.com/ongaeshi/ofruby-sample/blob/master/match3.rb)になりました。途中整形処理などにPCを使いましたが基本的にはコタツの中でiPod Touchを使って書きました。持ち運べるので、ちょっとした空き時間でコツコツ出来るのがミニコンピュータのよい所です。

とりあえずサンプルコードをコピペしてofrubyに貼付ければ遊べるようになるので遊んでみて下さい。レベル5とレベル10に壁があるのでまずはレベル10を目指すのがよいです。私のハイスコアは34000ですがその後まったく30000点を超えられなくなったのでバグかもしれません・・。

[f:id:tuto0621:20150217003856p:plain]

ゲームに飽きたらハイスコアをねつ造してスクリーンショットを取ったり6x6列を7x7列にしたり宝石の種類を増やしたり減らしたりするとよいと思います。

[f:id:tuto0621:20150217003904p:plain]








