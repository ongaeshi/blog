---
Title: ' Honyomi 0.2 リリース - キーワードハイライト、検索クエリ引き継ぎ'
Category:
- groonga
- ruby
- honyomi
- book
Date: 2014-10-08T00:15:34+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-0.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450067939809
---

Honyomi 0.2 をリリースしました。

主にWebアプリの使い勝手を改善しました。検索クエリが勝手に消えたりキーワード位置が見にくかった問題を改善しています。

* 検索結果の改善
  * キーワードをハイライト
  * 検索クエリの引き継ぎ
* テキスト
  * 'raw'から'Text'に名前変更
  * &lt;pre&gt;から&lt;div&gt;を使ってテキストを見やすく
  * キーワードをハイライト
  * 検索クエリの引き継ぎ

Honyomiは電子書籍(pdf)の検索エンジンとWebアプリです。手元にある大量のpdfをコマンドラインから登録し、ブラウザ経由で簡単に検索することが出来ます。

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

## 例えばこんな使い方
江添さんの[C++11の文法と機能](http://ezoeryou.github.io/cpp-book/C++11-Syntax-and-Feature.xhtml)をpdfに変換して少しずつ読んでいます(OSXだとWebページを印刷プレビュー→pdfに変換するのがなかなか優秀です)。nullptrなど興味のあるキーワードで検索してトピック単位でよめるのが便利です。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141008/20141008001504.png" alt="f:id:tuto0621:20141008001504p:plain" title="f:id:tuto0621:20141008001504p:plain" class="hatena-fotolife" itemprop="image"></span></p>

