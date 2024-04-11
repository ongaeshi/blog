---
Title: iOSアプリにRubyのシンタックスハイライトを組み込んだ
Category:
- ios
- ruby
- openframeworks
- ofruby
Date: 2015-05-17T17:55:43+09:00
URL: https://ongaeshi.hatenablog.com/entry/builtin-ruby-syntax-hight-to-ios
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450094706822
---

[ofruby](http://ofruby.tokyo/)にシンタックスハイライトを組み込んだ。

[f:id:tuto0621:20150517175305p:plain]


iOS7以降だったら自動でソースコードが色付けされるようになる。UITextViewに色々カスタマイズ出来るようになった(TextKit)のがiOS7以降なので残念ながらiOS6は今までどおり。

## 情報源
特に以下のブログが助けになった。

- [One-line fix for UITextView on iOS 7 - 24/7 twenty-four seven](http://kishikawakatsumi.hatenablog.com/entry/20140115/1389753015)
  - 貴重な日本語
- [Getting to Know TextKit - iOS 7 - objc.io issue #5](http://www.objc.io/issue-5/getting-to-know-textkit.html)
  - iOS7のTetKitを使ったシンタックスハイライトのサンプルコードが大変助かった
- [Making the Most of UITextView in iOS 7: NSTextStorage, NSLayoutManager, NSTextContainer and NSAttributedString](http://sketchytech.blogspot.com/2013/11/making-most-of-uitextview-in-ios-7.html)
  - 同じシンタックスハイライト実装のサンプルコード 

最終的に使ったライブラリ

- 1 [ICTextView](https://github.com/Exile90/ICTextView) (UITextViewの代わりに使うと変なバグが色々直る)
- 2 [RegexHighlightView](https://github.com/KayK/RegexHighlightView) (主にシンタックスハイライト用の正規表現を使わせてもらっている)

## 作戦
- UITextViewの代わりにICTextViewを使って細々としたバグを直す
  - UITextViewをカスタマイズしようとすると挙動が本当にびっくりするくらいおかしくなる(Apple頑張ってほしい)
  - いくつか試した感じICTextViewが一番安定していた(画面の一番上にカーソルを移動させたときの挙動とか完全には直ってないけど)
  - 他によいUITextViewカスタマイザーがあったら教えてください
- 「Getting to Know TextKit」を参考にRubyシンタックスハイライト用のカスタムTextStorageを実装
  - `RubyHighlightingTextStorage.m`と`EditViewController.mm `が処理の中心

詳しく知りたい人は先に情報源を読んでから[feature/code-highlight](https://github.com/ongaeshi/ofruby-ios/commits/feature/code-highlight)ブランチのコミットログを読むととよい。

## 入れてみた感想
やっぱり色は大事。小さな画面でも色付けされているとぱっとコード全体の構造が把握出来るようになった。文法エラーも発見しやすい。

ソースコードが色付けされて[Bluetoothキーボードを使う]()と大分PCに近づいた気がする。いつでもどこでもコードが書ける、思いついたことをすぐにコードに落とし込める、という意味ではPCに比べてアドバンテージがあるのでエディタ機能の改善は引き続き行っていきたい。

オートインデントとかオートコンプリージョンとかが次の候補だろうか。
