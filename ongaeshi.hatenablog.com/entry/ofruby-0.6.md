---
Title: iPhoneで実際に動くサンプルコードを見ながらRubyを使ってゲームを作れるように - ofruby 0.6
Category:
- ofruby
- openframeworks
- ruby
- ios
- iphone
- mruby
Date: 2015-03-05T01:16:36+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-0.6
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450086993959
---

ofruby 0.6 をリリースしました

## 新機能

[f:id:tuto0621:20150305011534p:plain]

[f:id:tuto0621:20150217001309g:plain]

- サンプルタブを追加
  - アプリ内で閲覧、実行出来るように
- 実際に遊べるゲームのサンプルコードを追加
  - 3マッチゲームやブロックくずしのサンプルコードを収録 
- ファイルを削除出来るように
  - 削除ボタンの追加
- バグ修正
  - iOSやiPadで上手く動かなかったりするのを修正しました

ダウンロードはこちらからどうぞ。

## 3マッチパズル
[f:id:tuto0621:20150217003856p:plain]

ofrubyをインストールしたらサンプルタブを開いて`match3.rb`を実行してみてください。
レベル5とレベル10辺りに壁があります。

## タッチした場所からボールが落ちる
<iframe src="https://youtube.googleapis.com/v/1P3dx6WkQ5U&amp;source=uds" allowfullscreen="" frameborder="0" height="315" width="420"></iframe>

`touch_fall.rb`です。スクリプトの内容をコピーしてファイルタブに追加し、定数NUMを

```diff
-NUM = 1
+NUM = 100
```

とかに変えてみると楽しいです。

## Links

- [iPhoneでRubyを書いてマッチ3パズル作った](http://ongaeshi.hatenablog.com/entry/ofruby-match3)
- [ofruby 0.3 をリリース - translate, rotate, scale が使えるようになりました](http://ongaeshi.hatenablog.com/entry/ofruby-0.3)
- [ofruby 0.2 リリース - タッチ入力が取得出来るようになりました](http://ongaeshi.hatenablog.com/entry/ofruby-0.2)
- [iPhoneでRubyとopenFrameworksを使ってグラフィックプログラミングができるofrubyを作りました - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/ofruby-released)
- [ongaeshi/ofruby-ios](https://github.com/ongaeshi/ofruby-ios)
