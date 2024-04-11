---
Title: ' 行指向のソースコード検索エンジンMilkode1.0.0リリース! '
Category:
- milkode
- groonga
- ruby
Date: 2013-05-23T13:57:13+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.0.0
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318753738949
---

**1.0.0** になりました。初のメジャーリリースバージョンとなります。

- ホームページリニューアル
- 統計情報に拡張子絞り込みのリンクを追加
- 1.0.0rc1で既に組み込まれている機能については[こちら](http://ongaeshi.hatenablog.com/entry/2013/05/08/170228)をどうぞ

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.0.0)

## ホームページリニューアル
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130519/20130519235217.png" alt="f:id:tuto0621:20130519235217p:plain" title="f:id:tuto0621:20130519235217p:plain" class="hatena-fotolife" itemprop="image"></span></p>

MilkodeのホームページをJekyllでリニューアルしました。<br>
[[詳しくみる]](http://ongaeshi.hatenablog.com/entry/software-page-renewed-using-jekyll)

## 統計情報に拡張子絞り込みのリンクを追加
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130523/20130523013645.png" alt="f:id:tuto0621:20130523013645p:plain" title="f:id:tuto0621:20130523013645p:plain" class="hatena-fotolife" itemprop="image"></span></p>

統計情報から拡張子絞り込みのためのリンクを追加しました。<br>
`.src`などの未登録な拡張子も表示するようにしました。

## リリースノート
* milk web
  * 統計情報
    * 拡張子絞り込みのリンクを追加
    * unknownの表示形式を変更
      * 旧: 'hoge.list' -> 'unknown'
      * 新: 'hoge.list' -> '.list'

* gmilk
  * Windows環境でgmilkが動かない問題を修正

* etc  
  * test/unit 2.5.4 で警告が出る箇所を修正

## 関連記事
- [gihyo.jpに記事を書いた時の感想](http://ongaeshi.hatenablog.com/entry/write-article-on-gihyojp)
- [0.9.9.9(実質1.0.0.rc1) - Ruby2.0対応とか](http://ongaeshi.hatenablog.com/entry/2013/05/08/170228)
- [0.9.9 - 複数行の検索に対応、Emacsからファイルを直接開けるように](http://ongaeshi.hatenablog.com/entry/2013/03/28/152538)
- [エディタとの連携 - ブラウザで見ているソースコードのファイルをエディタから一瞬で開く](http://ongaeshi.hatenablog.com/entry/2013/03/25/175031)

## 終わりに
[2011年7月に0.1.0をリリース](https://github.com/ongaeshi/milkode/blob/master/HISTORY.ja.rdoc)してから二年近く経とうとしています。こんなに長く1つのソフトを開発し続けるのは始めてかもしれません。

こんなに長く続けられたのは、使ってくれたり、要望やバグ報告やパッチをくれたり、ブログやTwitterなどでMilkodeに言及してくれる皆様のおかげです。あらためてありがとうございます。

幸いまだやりたいことが残っているのでMilkode1.1に向けて開発を続ける予定です。要望の多いサブURLの対応とせっかくここまで作ったので英語化は最低限したいなあとか思っています。



