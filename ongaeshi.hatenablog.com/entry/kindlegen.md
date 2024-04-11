---
Title: epubからmobiに変換するならkindlegenが便利
Category:
- kindle
Date: 2017-02-06T22:58:19+09:00
URL: https://ongaeshi.hatenablog.com/entry/kindlegen
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687214181215
---

[購入したepub](http://tatsu-zine.com/books/scheme-in-ruby)をKindleに送ろうと思ったら、数ヶ月前にPCを乗り換えたのでcalibreがインストールされていなかった。

しかたないので[昔書いた記事](http://ongaeshi.hatenablog.com/entry/2013/03/12/151447)を見ながらcalibreを再インストールする。意外とブクマついてたので覗いてみると

> 変換だけなら公式のkindlegenの方が手軽かと。
http://b.hatena.ne.jp/entry/136255446/comment/hageatama-

これは便利そうなので試してみよう。

## kindlgenのインストール
[KindleGen v2.9](https://kdp.amazon.co.jp/help?topicId=A3IWA2TQYMZ5J6)をダウンロードして

```
$ unzip KindleGen_Mac_i386_v2_9.zip 
$ mv kindlegen /usr/local/bin/
```

これでインストールは終了。Windows, Linux, MacOS 用が用意されている。

## epub -> mobi

```
$ kindlegen scheme-in-ruby.epub 
*************************************************************
 Amazon kindlegen(MAC OSX) V2.9 build 1028-0897292 
 A command line e-book compiler 
 Copyright Amazon.com and its Affiliates 2014 
*************************************************************

Info(prcgen):I1047: Added metadata dc:Title        "つくって学ぶプログラミング言語　RubyによるScheme処理系の実装"
Info(prcgen):I1047: Added metadata dc:Date         "2013-04-16"
Info(prcgen):I1047: Added metadata dc:Creator      "渡辺昌寛"
Info(prcgen):I1047: Added metadata dc:Publisher    "達人出版会"
Info(prcgen):I1047: Added metadata dc:Contributor  "高橋征義"
.
.
Info(prcgen):I1036: Mobi file built successfully

$ ls
scheme-in-ruby.epub
scheme-in-ruby.mobi
```

完成、あとはメールにmobiを添付してKindleに送りつければよい。

コマンドラインでさくさくepubからmobiに変換できるとやはり楽でよい。
