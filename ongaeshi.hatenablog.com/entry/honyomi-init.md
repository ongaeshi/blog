---
Title: 数万の電子書籍から目的のページを一瞬で見つけ出す、Honyomi
Category:
- ruby
- groonga
- honyomi
- milkode
Date: 2014-04-10T10:53:45+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-init
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815721665198
---

<span style="font-size: 150%">[続き](http://ongaeshi.hatenablog.com/entry/future-of-ebook-and-search-engine)を書きました。</span>

![honyomi.gif](https://github.com/ongaeshi/honyomi/raw/master/images/honyomi-01.png)

[ongaeshi/honyomi](https://github.com/ongaeshi/honyomi)

Honyomiは個人やイントラネット環境で使える電子書籍(pdf)の検索エンジンとWebアプリです。手元にある大量のpdfをコマンドラインから登録し、ブラウザ経由で簡単に検索することが出来ます。

Honyomiは[Milkode](https://github.com/ongaeshi/milkode)の電子書籍版ともいえます。使い方も似ているため、Milkodeを使ったことのある人はよりスムーズに使えるのではないかと思います。

## 作った経緯

紙の本も好きなのですが、電子書籍で購入したり、本棚整理時に自炊することが増えてきました。

で、ふとあの時に読んだあれはどこにあったっけ、となっても見つけられないことが何度か起きました。紙の本であれば背表紙から本を探してぱらぱらとめくって見つけることが出来るのだけど、ファイル名だけどなんとなく勘が働かない・・。やはりデジタルデータは検索エンジンから検索出来るようにするのがよさそうだと思い、作ってみました。

手元に大量の電子書籍を持っている方は是非使ってみて下さい。いつでも過去に読んだ本を検索で読み返せるようになると読書体験が一段階パワーアップするかもしれません。

社内文書がpdfで配布されているような環境でも便利に使えるのではないかと思います。

## インストール

    $ gem install honyomi

Rroongaのインストールに失敗する場合はこちらを参考にして下さい。

* [File: install — rroonga - Ranguba](http://ranguba.org/rroonga/ja/file.install.html)

また、それ以外に以下のツールが必要です

* pdftotext - pdfの読み込みに使います (poppler, xpdf)

## 使い方

### データベースの作成

```
$ honyomi init
Create database to "/home/username/.honyomi/db/honyomi.db"
```

データベースを指定 (環境変数`HONYOMI_DATABASE_DIR`を設定、他のコマンドでも同様に動作します)

```
$ HONYOMI_DATABASE_DIR=/path/to/dir honyomi init
Create database to "/path/to/dir/db/honyomi.db"
```

### 本の追加

```
$ honyomi add /path/to/this_is_book.pdf
A 1 this_is_book (10 pages)
```

### 本の編集

idを指定してタイトルを変更します。

```
$ honyomi edit 1 -t "This is Book"
id:        1
title:     This is Book
path:      /path/to/this_is_book.pdf
pages:     10
timestamp: 2013-01-01 00:00:00
```

### 本を一覧

```
$ honyomi list
1 This is Book (10 pages)
2 That is Book (20 pages)
```

idを指定して詳細を表示します。

```
$ honyomi list 1
id:        1
title:     This is Book
path:      /path/to/this_is_book.pdf
pages:     10
timestamp: 2013-01-01 00:00:00
```

### コマンドラインから検索

```
$ honyomi search bbb
1 matches
--- This is Book (5 page) ---
aaa <<bbb>> ccc
```

### Webアプリを起動

```
$ honyomi web
```

* AND, OR, NOT検索
* `Filter+, -`による絞り込み
* `Download`でダウンロード(サイズの大きなpdfに使うと便利、OSXだと[Skim](http://skim-app.sourceforge.net/)で読むのがおすすめ、Windowsでいいやつ教えて下さい)
* `Pdf`でインライン表示
* `Raw`でテキストだけを表示

など一通りの機能は揃っています。

![honyomi-03.gif](https://github.com/ongaeshi/honyomi/raw/master/images/honyomi-03.gif)


