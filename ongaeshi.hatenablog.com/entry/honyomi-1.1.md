---
Title: Honyomi 1.1 - Webアプリからドラッグ＆ドロップで書籍が追加できるようになりました
Category:
- honyomi
- groonga
- ruby
- book
Date: 2015-06-26T01:33:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450098975927
---

Honyomiは電子書籍(pdf)の検索エンジンとWebアプリです。手元にある大量のpdfをコマンドラインから登録し、ブラウザ経由で簡単に検索することが出来ます。

[f:id:tuto0621:20150626013228p:plain]

- [リリースノート](https://github.com/ongaeshi/honyomi/blob/master/HISTORY.md#11---2015-06-23)

## インストール

    $ gem install honyomi

Rroongaのインストールに失敗する場合はこちらを参考にして下さい。

* [File: install — rroonga - Ranguba](http://ranguba.org/rroonga/ja/file.install.html)

また、それ以外に以下のツールが必要です

* pdftotext - pdfの読み込みに使います (poppler, xpdf)

詳しい使い方は以下をどうぞ。

[ongaeshi/honyomi](https://github.com/ongaeshi/honyomi)

認証をかけて手持ちのpdfをインターネット上に置くことも出来ます。自分の持っている全てのpdfをいつでも検索、閲覧が可能になります。

[ongaeshi/honyomi-web](https://github.com/ongaeshi/honyomi-web)


## Webアプリからドラッグ＆ドロップで書籍を追加

[f:id:tuto0621:20150626013240p:plain]

コマンドラインを使わずに書籍を追加できるようになりました。

無効にしたい場合は環境変数`HONYOMI_DISABLE_WEB_ADD`を設定して起動してください。

    $ HONYOMI_DISABLE_WEB_ADD=1 honyomi web




