---
Title: 英語力0から洋書を読むだけで英語を学んでいくリスト その2
Category:
- english
Date: 2016-09-04T23:40:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/list-of-english-books
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687178200732
---

[旧ページ](http://qiita.com/ongaeshi/items/5167f020f1879968a3e6)から引っ越しました。

本当に読みたい洋書だけを読んで英語力を向上させていく取り組みです。
はるか昔にやったTOEICは400点くらいだった記憶があります。

[:contents]

## 役に立つ基礎知識
- epubは[calibre](http://ongaeshi.hatenablog.com/entry/2013/03/12/151447)を使うことでKindleで読めるmobiに変換できます。
- Send to Kindle を使えばmobiを添付してメールするだけで自分のKidleに書籍を転送することができます

## (読書中: 10%) Politics and the English Language
[Politics and the English Language](https://www.amazon.co.jp/dp/B00AZQTM5I)

ジョージ・オーウェルのエッセイ。「政治と英語」。

## Producing Open Source Software
[Producing Open Source Software](http://producingoss.com/)

(購入) - 2016-08-12 **無料**

日本語タイトル「オープンソフトウェアの育て方」。日本語のepubもある！と喜んだが残念ながらうまく開けなかった(ドイツ語や中文も無理だった)。仕方がないので英語版のepubをダウンロード。

日本語版も[Webからは読める](http://producingoss.com/ja/index.html)模様。

(追記) 以下の手順でepubを修正すれば日本語版も読むことができる(thx: [@icm7216](https://twitter.com/icm7216))

[https://twitter.com/icm7216/status/763933421714280448:embed]
[https://twitter.com/icm7216/status/763936567069057027:embed]

## Spark
[Spark: 17 Steps That Will Boost Your Motivation For Anything (ebook) - PsyBlog](http://www.spring.org.uk/spark-how-to-get-motivated) **$10.00**

(購入) - 2016-08-05

[PSYBLOG](http://store.toyokeizai.net/books/9784492045510/)の人が書いた本。短くて安いので買ってみる。

## Create Your Own Programming Language
[Create Your Own Programming Language](http://createyourproglang.com/) **$39.99**

たった100Pでプログラミング言語の作り方の基礎が学べる。

感想 - [自分でプログラム言語を書いてみたい人は「Create Your Own Programming Language」がおすすめ - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/create-your-own-programming-language)

## REWORK
[REWORK](https://books.wikihub.io/wiki/REWORK) **￥1,400**

(購入) 2016-05-01

日本語訳もあるけど、37signalsっぽい雰囲気を生で感じてみたかったので原著で。

自分が作ったものの過程で学んだ副産物もコンテンツだからちゃんと公開しろよ！(意訳)」みたいなトピックが妙に心に残った。(だからこんな記事書いてる)

## iOS 9 SDK Development
[The Pragmatic Bookshelf | iOS 9 SDK Development](https://pragprog.com/book/adios3/ios-9-sdk-development) **$27.00**

(購入) 2016-01-03

ずっとiOS5辺りの古い知識のままiOSアプリを作っているのでそろそろ刷新しようと思い購入。

dispatch_asyncを使った非同期処理の書き方とかかなり参考になった。

## Mastering Emacs
[The Mastering Emacs Ebook](https://www.masteringemacs.org/order) **$20.00**

(購入) 2015-11-28

セール中で20$だったので買った。ブログも良い情報が多い。

感想 - [Mastering Emacsのすすめと、意外と便利な数引数の話 - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/mastering-emacs-and-number-argument)

## The Rust Programming Language
[Download 'The Rust Programming Language' E-Books (PDF, EPUB, MOBI)](https://killercup.github.io/trpl-ebook/) **無料**

[公式マニュアル](http://doc.rust-lang.org/nightly/book/)をebook化したもの。epub版を[calibre](http://ongaeshi.hatenablog.com/entry/2013/03/12/151447)でmobiに変換して読んでます。

読了、メモリとポインタ周りの話が面白かったです。GCを入れてシンプルに解決するGoと違い、たくさんの仕組みを導入して泥臭く(そして実行時オーバーヘッドを最小にして)解決しようとする姿勢が良いです。

## ソフトスキル - ソフトウェア開発者のライフマニュアル
[Soft Skills](https://www.manning.com/books/soft-skills) **$27.99**

- [Higepon’s blog](http://d.hatena.ne.jp/higepon/20150921/1442843666)を見て面白そうだったので購入

## Elixirでプログラミング
[Programming Elixir](https://pragprog.com/book/elixir/programming-elixir) **$24.00**

- 並列プログラミングに特化したプログラミングの概念がざっくりと分かった
- async-await (C#)やActor (Scala) いったものがElixir(というかErlangVMの)の強力な並列性の上にライブラリとして作られている所。
  - async-awaitやActor,Taskはあくまで「よく使うライブラリ」
  - その原子にあるのは「軽量で安全なメッセセージパッシングでやりとりするErlangプロセス」ということがなんとなく分かった気がする。
- Elixirは見た目がRubyに似ているので読みやすくて助かった。
- attr_reader がないとか、Rubyいいけどここはこっちの方がよくない？みたいな「オレオレRuby」感があるのも結構好き。(私はattr_reader好きだけど)
  - module A.B.Math … end みたいにモジュール名を名前空間付きで一行で定義できるのはいいなと思った。

## 健康なプログラマー
[The Healthy Programmer](https://pragprog.com/book/jkthp/the-healthy-programmer)

- サンプルコードのない本を読めるかチャレンジ
- 結構大変だったがなんとか読めた(時間はいままで一番かかった)
- 日本語書籍の薄さを見て(こんなに英語だと時間がかかるものかと)愕然とした

## Rubyでテキスト処理
[Text Processing with Ruby](https://pragprog.com/book/rmtpruby/text-processing-with-ruby)

- 自分のそこそこ詳しいことなら英語でも読みやすいんじゃないか作戦
- うまくいった、半分くらいはもう知っていることだったので半分を予測しながら読める
- Gitのプログレスバーみたいなやつを再現するにはstderrを使えばよい、ということが分かったのが最大の収穫

## Rubyでゲームプログラミングを学ぶ
[Learn Game Programming with Ruby](https://pragprog.com/book/msgpkids/learn-game-programming-with-ruby)

- Rubyでゲームプログラミングを学ぶ本
- ゲームだと画像も多めで読みやすいじゃないかという予測→当たった！
- もしかしたら子供も読むことを想定して語彙も簡単にしてくれているのかもしれない

## 使用中の英文法
[English Grammar in Use](http://www.amazon.co.jp/English-Grammar-Use-Answers-CD-ROM/dp/052118939X)

- まずは最低限の英文法を覚える
- 英文法を覚えるために英語で書かれた英文法の本を読むのおすすめ
- [感想1](http://ongaeshi.hatenablog.com/entry/homework-of-winter-break), [感想2](http://ongaeshi.hatenablog.com/entry/english-grammer-in-use-1) 全部は終わらなかった、全体の2/3くらいで飽きたので実践形式で読みたい本を読んでいくことにした

## 更新履歴
- 2016-08-09 引っ越し
- 2015-09-26 新しいものを一番上に置くようにしました
- 2015-09-26 ソフトスキル -ソフトウェアでデベロッパーのライフマニュアル- 追加
- 2015-09-26 Programming Elixir読了
