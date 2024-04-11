---
Title: 理想の読書システムが構築できたので紹介します
Category:
- honyomi
- diary
- book
- groonga
- ruby
- programming
Date: 2015-10-27T23:25:38+09:00
URL: https://ongaeshi.hatenablog.com/entry/ideal-reading-system
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415126050143
---

電子書籍、全文検索、Webアプリケーションといった現代技術を組み合わせる(無いものは自分でソフトウェアを書いた)ことで理想の読書システムを構築することができた気がしたので紹介します。

## 1. 購入
コンピュータ関連の書籍は [Gihyo](https://gihyo.jp/dp)、[オライリー](https://www.oreilly.co.jp/ebook/)、[達人出版会](http://tatsu-zine.com/)などほとんどがDRMフリー(mobi, epub, pdf)で購入できる。後で検索できるようにしたいので、できるだけpdfがついてくるものを買う。

洋書は[The Pragmatic Bookshelf](https://pragprog.com/)を定期的にチェックしている。PayPalのアカウントを持っているとあまり知らない出版社から購入するときでも安心。

電子版がないけどPCで読みたいもの、最初はどうしても紙で読みたいものは自炊する。

## 2. 読む
本当は紙の本で読みたいのだが、ペーパーバック+電子書籍で安く買えるプランがあまりないので電子書籍で買ってKindleで読むことが多い。Send to Kindle(外のサイトで買った書籍をメールでKindleに送れる)が便利。

購入先でmobiがついてこない場合は[Calibre](http://ongaeshi.hatenablog.com/entry/2013/03/12/151447)を使ってepubからmobiに変換する。

大きくて読みやすいのでメインはPaperwhite。とはいえ読みたい時にどの端末を持っているか分からないので、iPod touchにもAndroid端末にもKindleアプリは入れてある。自転車で遠出する時などはサドルの荷物入れにPaperwhiteが入らないのでiPod touchで読む。

## 3. 検索
![honyomi-demo](https://raw.githubusercontent.com/ongaeshi/honyomi/master/images/honyomi-03.gif)

自作ソフトウェアの[Honyomi](http://honyomi.nagoya/)を使う。Conohaに自分用のHonyomiサーバーを[認証をかけてデプロイ](http://ongaeshi.hatenablog.com/entry/honyomi-1.4)してあるのでKindleにメールを送るタイミングと一緒にpdfを追加しておく。サーバーに置くほどでなければgemやDockerコンテナ(おすすめ)からローカルマシンに[インストール](http://honyomi.nagoya/ja/install.html)しても良い。

メモを書く場合はHonyomiに残しておくと後から検索しやすい。とりあえずKindleにラインやメモを残して後でHonyomiに移植したりもする。

読み終わった本を読み返すときはHonyomiを使う。なんとなく覚えている単語を入力すれば目的のページをだいたい見つけることができる。ページは画像化されているのでその場ですぐに内容を確認できる。1回でも読み返したページにはブックマークをつけておく。

[f:id:tuto0621:20151027232500p:plain]

単語で見つからなかった時は目次のページを読んでページ番号を入力する(Honyomiは数字単独で検索すると指定ページへのジャンプになる)。さすが本は目次と索引がとても綺麗にまとまっているので全文検索か目次、索引をざっと見ることで目的の内容はだいたい見つけることができる。

Honyomiが特に力を発揮するのはレシピブック系。[Rubyレシピブック](http://www.amazon.co.jp/dp/4797359986)や[Ruby逆引きハンドブック](www.amazon.co.jp/dp/4863540221)には何度もお世話になっている(電子書籍版がないので自炊、pdf版が発売されるのをずっと待っています)。あるメソッドの使い方を調べたいときなどリファレンスマニュアルよりも情報量が多くて理解が捗る。あとは[リーダブルコード](http://www.amazon.co.jp/dp/4873115655)とか。"名前"、"境界値"、"min max"とかで何回も読み返している。

[f:id:tuto0621:20151027232513p:plain]

ある程度理解したら1.に戻って次の本の物色の旅へ・・。

<b>追記</b>

- Honyomiはここでお試しできます [本読みの図書館](http://library.honyomi.nagoya/) 

## まとめ

1回読んだだけではあまり効果のない、いわゆる本棚にずっとおいて置きたい系の本とHonyomiを使った全文検索サービスの相性はとても良いと感じる。興味が沸いたらぜひDRMフリーの書籍を購入して自分用の書籍データベースを構築してみて欲しい。

