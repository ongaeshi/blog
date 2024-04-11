---
Title: 簡易認証を使ってインターネット上に購入した電子書籍を置く - Honyomi 1.4
Category:
- honyomi
- groonga
- ruby
- ebook
- pdf
Date: 2015-09-29T01:05:21+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415122929474
---

※ Kitematicがある人はすぐに手元で動かすことができます

![install-demo](https://raw.githubusercontent.com/ongaeshi/honyomi/master/images/honyomi-03.gif)

[Honyomi](http://honyomi.nagoya/)1.4で簡易認証機能を追加しました。Webサーバー側の設定不要でパスワードロックが可能です。

達人出版やGihyo Digital、Pragmatic Bookshelfで購入した本もインターネットに置けるようになります。

## インストール

[ongaeshi/honyomi- Basic Authorization](https://github.com/ongaeshi/honyomi#basic-authorization)

環境変数を設定して認証をかけます。[Docker](https://github.com/ongaeshi/docker-honyomi)コンテナを使うと、

```
$ docker run --name my-honyomi -d -it -p 9295:9295 -v /path/to/honyomi:/root/.honyomi/ -e HONYOMI_AUTH_USERNAME=username -e HONYOMI_AUTH_PASSWORD=password_hexdigest ongaeshi/honyomi
```

とたったの1コマンドで

- pdfのインライン表示
- 高速な全文検索
- Webアプリからのアップロード
- パスワードロック
- データベースは`/path/to/honyomi`に作成

なpdf全文検索エンジンを立ち上げることができます。(デモ → [本読みの図書館](http://library.honyomi.nagoya/))

## ホームページ

日本語の情報が少なかったので追加しました。

[Honyomi - Rubyで書かれたpdfの全文検索エンジン](http://honyomi.nagoya/ja/)




