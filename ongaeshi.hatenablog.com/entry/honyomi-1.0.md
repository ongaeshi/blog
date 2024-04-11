---
Title: Honyomi 1.0 リリース - ブックマーク＆コメント、Web上で本の情報を編集、便利な検索クエリ (デモもあるよ！)
Category:
- ruby
- groonga
- hoyomi
- book
- web
Date: 2014-11-17T11:34:34+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.0
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450074004839
---

Honyomiは電子書籍(pdf)の検索エンジンとWebアプリです。手元にある大量のpdfをコマンドラインから登録し、ブラウザ経由で簡単に検索することが出来ます。

前回リリースの0.2から大きくジャンプアップして初のメジャーリリースとなります。欲しかった機能が一通り入った感じがしたので1.0リリースとなりました。

* ブックマーク＆コメント
* Web上で本の情報を編集
* 便利な検索クエリ 
* その他、全体的な使い勝手の改善

## デモ
以下のページを立ち上げました。

[本読みの図書館](http://honyomi.ongaeshi.me/)

再配布可能な電子書籍を集めて(置いてある書籍はどれも「これ無料で配布していいの？」って位クオリティが高いです
) 全文検索出来るようにしました。 Honyomiを使うとまったく同じ環境を手持ちのpdfをソースにして構築することが出来ます。

## ブックマーク＆コメント
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141116/20141116120834.gif" alt="f:id:tuto0621:20141116120834g:plain" title="f:id:tuto0621:20141116120834g:plain" class="hatena-fotolife" itemprop="image"></span></p>

気になったページにブックマーク＆コメントを付けられるようになりました。コメントに書かれた`P11`みたいのは自動的にページ内リンクになります。`http://`もリンクが貼られます。

コメントも検索対象に含まれるので読み途中の書籍に`ここまで読んだ`とブックマークを付けておくと読み返す時に便利です。

画像のpdfは[O'Reilly Japan - リーダブルコード](http://www.oreilly.co.jp/books/9784873115658/)のページから買えます。

## Web上で本の情報を編集
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141116/20141116120655.gif" alt="f:id:tuto0621:20141116120655g:plain" title="f:id:tuto0621:20141116120655g:plain" class="hatena-fotolife" itemprop="image"></span></p>

コマンドラインを叩かずにタイトル、著者、URLを設定出来るようになりました。

## 便利な検索クエリ 
タイトルによる絞り込み指定ページへのダイレクトジャンプが出来るようになりました。

- `123`: 123ページにジャンプ
- `t:hello world`: タイトルで絞り込み
- `-t:hello world`: タイトルで絞り込み (NOT)

特に数値入力による指定ページへのジャンプ機能はとても便利です。pdf読んでて「ここブックマークしたい！」と思った時に使います。

詳しくは[Help](http://honyomi.ongaeshi.me/help)をどうぞ。

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

## どんな時に便利なの？

1つの電子書籍リーダーにロックインされずに本を読むことが出来ます。移動中はスマホで読むけど家ではタブレット、その後PCで作業中に「そういえばあの本に書いてあったな」とか結構ありませんか？

見た目は0.2と余り変わっていませんが中身は別物に使いやすくなりました。是非お試し下さい。
