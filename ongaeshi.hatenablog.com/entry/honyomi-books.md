---
Title: 全文検索可能な電子図書館を作ってみた
Category:
- ruby
- groonga
- web
- honyomi
- book
- 本
- diary
Date: 2014-11-18T01:34:47+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-books
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450074025889
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141116/20141116171416.gif" alt="f:id:tuto0621:20141116171416g:plain" title="f:id:tuto0621:20141116171416g:plain" class="hatena-fotolife" itemprop="image"></span></p>

本読みの図書館
http://honyomi.ongaeshi.me/

先日リリースした[Honyomi1.0](http://ongaeshi.hatenablog.com/entry/honyomi-1.0)をベースに作成しました。

再配布可能な電子書籍を集めて全文検索出来るようにしてあります。 
原文のpdfをその場で読んだりダウンロードしてオフラインで読むことも可能です。
気になったページにブックマークを付ける、メモを残す、他の人の書いたメモを読むことも出来ます。 

詳しい使い方は[このサイトについて](http://honyomi.ongaeshi.me/about)をどうぞ。

## 作った経緯
Honyomiという手持ちのpdfをまとめて検索したりメモを書けるツールを作っているのですが、自力で書籍サーバーを立ち上げて自分の蔵書を管理するのはやっぱり大変なので、やろう！って思ってもらえるように実際に動くものを立ち上げたいと思っていました。

立ち上げるにあたって「何の本を置くか」というのが一番大切で自分が個人で使っている(有料の書籍がたくさん入った)Honyomiサーバーを見せればみんな使ってみたい！と思ってくれるのは確実なのですがそういう訳にもいかないし、かといって「あいうえお」みたいなサンプル本を置いても使ってみようという気分になりません。

調べてみるとクリエイティブコモンズやGFDL(GNU Free Documentation License)といった書籍はそこそこあり、それらを集めて検索出来るようにしたら面白いのではないかと思いはじめてみることにしました。

他にも再配布可能なライセンスの電子書籍を知っている人がいましたら是非教えて下さい。

## 図書館とソーシャル性
一冊の本を大勢で読んだりメモを書けるようにしたらどんなソーシャル効果が生まれるのかというのも少し興味があります。<b>未来の図書館を作るとは</b>でも図書館の特徴にソーシャル性を挙げていますね。

[未来の図書館を作るとは(P7)](http://honyomi.ongaeshi.me/v/1?query=%E7%9F%A5%E3%81%AE%E5%85%B1%E6%9C%89&page=7)
> 個人がぼう大な数の書物を集めて自分の思想形成のために使うという場合にくらべて図書館には1つの本質的な違いがある。それは知の共有のシステムであるという所にある。
> これは考え方の違う人達が知識を共有し、その違いを議論を通じて明らかにすると共に、新しい知識・思想を作り出してゆく場を意味しており、これが図書館の真のあり方だと言えるのではないだろうか。

私個人としては、個人がぼう大な数の書物を集め思想形成に役立てるのも、知を共有することもまだまだ進化の余地があると思っており、そのためにHonyomiを作っていたりします。

## 自分用の書籍サーバーを立ち上げる
本読みの図書館を使ってみてよいと感じたら、是非[honyomi-web](https://github.com/ongaeshi/honyomi-web)などを使って自分が過去に買った書籍を[いつでも検索、閲覧](http://ongaeshi.hatenablog.com/entry/honyomi-web)出来るようにしてみてください。とても便利ですよ。




