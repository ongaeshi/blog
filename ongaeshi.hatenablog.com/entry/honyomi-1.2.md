---
Title: pdfをブラウザからインラインで閲覧できるようになりました - Honyomi 1.2
Category:
- ruby
- honyomi
- groonga
Date: 2015-07-05T17:45:54+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450100304517
---

Honyomi 1.2 をリリースしました。これは電子書籍検索エンジンとしての使い勝手を向上させる大きなリリースです。

<b><span style="font-size: 150%">pdfファイルのページ内容をブラウザ上で直接読める</span></b>できるようになりました。(今まではテキスト内容だけが表示されており実際に読むには元のpdfを別ツールなどで開く必要がありました)

[f:id:tuto0621:20150705173416p:plain]

検索結果にもヒットしたテキストと一緒にページが表示されるようになります。

[f:id:tuto0621:20150705173427p:plain]

## デモ
全ての書籍をインライン閲覧できるようにしてあります。

http://honyomi.ongaeshi.me/

[検索](http://honyomi.ongaeshi.me/?query=bisect)したり[ブックマークを閲覧](http://honyomi.ongaeshi.me/?b=1)したり色々お試しください。

## インストール

    $ gem install honyomi

Rroongaのインストールに失敗する場合はこちらを参考にして下さい。

* [File: install — rroonga - Ranguba](http://ranguba.org/rroonga/ja/file.install.html)

また、それ以外に以下のツールが必要です

* pdftotext - pdfの読み込みに使います (poppler, xpdf)
* pdftoppm - imageコマンドに必要です

詳しい使い方は以下をどうぞ。

[ongaeshi/honyomi](https://github.com/ongaeshi/honyomi)

認証をかけて手持ちのpdfをインターネット上に置くことも出来ます。自分の持っている全てのpdfをいつでも検索、閲覧が可能になります。

[ongaeshi/honyomi-web](https://github.com/ongaeshi/honyomi-web)

## imageコマンド

インライン閲覧を可能にするにはあらかじめimageコマンドを実行しておく必要があります。

書籍のidを調べて(Webアプリからも確認できます)、

```
$ honyomi list
 1 aaa (228 pages)
 2 bbb (210 pages)
 3 ccc (228 pages)
 .
 .
```

`honyomi image`コマンドで生成します。

```
$ honyomi image 1
Generated images to '/Users/ongaeshi/.honyomi/image/1'
```

imageコマンドには`pdftoppm`が必要ですが、比較的新しいOSなら標準で入っていたり`yum install poppler`などで一緒にインストールされていると思います。CentOS5の人は[こちら](http://qiita.com/ongaeshi/items/e7ef572a1774c926d05d)を参考にしてください。

## 感想
正直これ別ツールなんじゃないか、ってくらい便利になりました。

[フクオカRuby大賞本審査](http://ongaeshi.hatenablog.com/entry/fukuoka-ruby-07)のささださんの宿題にも少しだけ応えられたんじゃないかと思います(実際のページを読みながらブックマークに付けたコメントも読むことができるようになりました)。

インライン閲覧するためのよい方法が思いつかずHonyomiの開発はずっと止まっていたのですが、今回[@y_jonoさん](https://twitter.com/y_jono)さんがHonyomiのバグ報告を送ってくださり、それを直している最中にpdftoppmと組み合わせる方法が急に降りてきました。やはりユーザーさんを大切にするといいことがありますね😎




