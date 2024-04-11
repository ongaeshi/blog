---
Title: pdfをブラウザにドラッグ＆ドロップして全文検索しながら読む - Honyomi1.3
Category:
- docker
- honyomi
- ruby
- groonga
Date: 2015-07-24T23:34:08+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450103022876
---

Honyomi 1.3 をリリースしました。pdftoppmがインストールされていればドラッグ＆ドロップするだけですぐにインライン閲覧できるようになりました。

[f:id:tuto0621:20150724233354p:plain]

割と理想のpdf全文検索アプリになってきたんじゃないかと思います。いちいちpdf本体を開かずに読んだり検索できるので便利です。

## デモ
全ての書籍をインライン閲覧できるようにしてあります。

http://honyomi.ongaeshi.me/

[検索](http://honyomi.ongaeshi.me/?query=bisect)したり[ブックマークを閲覧](http://honyomi.ongaeshi.me/?b=1)したり色々お試しください。

## インストール

Dockerが使える人用にビルド済みコンテナも用意しました。すぐに使えるのでおすすめです。

- [Docker経由でインストールする](http://ongaeshi.hatenablog.com/entry/docker-honyomi)

今まで通りgem経由でインストールすることも可能です。

    $ gem install honyomi

Rroongaのインストールに失敗する場合はこちらを参考にして下さい。

* [File: install — rroonga - Ranguba](http://ranguba.org/rroonga/ja/file.install.html)

また、それ以外に以下のツールが必要です

* pdftotext - pdfの読み込みに使います (poppler, xpdf)
* pdftoppm - imageコマンドに必要です

詳しい使い方は以下をどうぞ。

[ongaeshi/honyomi](https://github.com/ongaeshi/honyomi)

認証をかけて手持ちのpdfをインターネット上に置くことも出来ます。自分の持っている全てのpdfをいつでも検索、閲覧が可能になります。(今となってはDockerコンテナ使った方が楽かもしれません)

[ongaeshi/honyomi-web](https://github.com/ongaeshi/honyomi-web)




