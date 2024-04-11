---
Title: ofrubyのはじめかた
Category:
- ofruby
- ruby
- openframeworks
- ios
- mruby
Date: 2015-05-20T23:57:08+09:00
URL: https://ongaeshi.hatenablog.com/entry/start-ofruby
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450095047313
---

紹介されてた。

[ofruby – iPhoneで動くRubyを使ったグラフィックプログラミング環境](http://www.softantenna.com/wp/iphone/ofruby/)

インストールして最初に何をすればよいか分かりにくいと思うのでおすすめのはじめかたを書いてみます。

## サンプルコードの実行
[f:id:tuto0621:20150305011534p:plain]

起動すると[File]と[Sample]の2つのタブがあるので、[Sample]をクリックしてサンプルを上から順に実行していきます。今のofrubyでどんなことが出来るか分かると思います。

※ バージョンアップ時に新機能が入った時はそのサンプルコードが増えたりするので参考にしてください。

## サンプルコードを改造
気に入ったものがあったらそれをベースに色々改造してみます。サンプルコードは編集出来ないので[File]領域にコピーします。

```
サンプルコードをコピー
↓
[File]タブに移動、[+]を押す
↓
ファイル名を指定してファイル作成
↓
全選択、貼付け (中身を丸ごと入れ替え)
↓
実行
```

これでサンプルコードと同じプログラムが動くようになるはずです。ここから改造してみましょう。はじめは数値を変更してプログラムにどのような変化が起きるか試してみることからはじめるとよいです。

## もっとサンプルコードが欲しい
ofruby-sampleがよいです。

- [ongaeshi/ofruby-sample](https://github.com/ongaeshi/ofruby-sample#striperb)

こちらは旧バージョンの[RubyKokuban](http://ongaeshi.hatenablog.com/entry/rubykokuban-sketch)のものですが少し修正(特に入力周りをマウスからタッチに)すれば動くかもしれないです。

- [ongaeshi/rubykokuban-sample](https://github.com/ongaeshi/rubykokuban-sample)

## 情報源
このブログのofrubyカテゴリーを読んでいくとよいです。ここで紹介されているサンプルコードもコピペすれば動かすことが出来ます。とにかくまずは動くものをコピペ、リミックスしていく所からはじめていくとよいです。

- [ofruby カテゴリーの記事一覧 - ブログのおんがえし](http://ongaeshi.hatenablog.com/archive/category/ofruby)

## Ruby
mrubyベースですが基本的な文法はRubyと同じなのでRubyのWeb上の情報が参考になると思います。

リファレンスマニュアルのおすすめ。

- [オブジェクト指向スクリプト言語 Ruby リファレンスマニュアル](http://miyamae.github.io/rubydoc-ja/2.2.0/#!/doc/index.html)

書籍のおすすめ。

[asin:4797372273:detail]

## 作ったコードをシェアしたい
https://github.com/

一報いただけたら大変喜びます🙇 分かりにくい点がありましたらTwitterなどで聞いてください。

