---
Title: ' 自作ソフトのホームページをMediaWikiからJekyllに移行した'
Category:
- milkode
- ruby
- groonga
- jekyll
Date: 2013-05-20T13:27:51+09:00
URL: https://ongaeshi.hatenablog.com/entry/software-page-renewed-using-jekyll
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318753640844
---

MilkodeのホームページをJekyllでリニューアルしました。

[Milkode - 行指向のソースコード検索エンジン](http://milkode.ongaeshi.me/)
 
<span style="font-size: 80%">※ 旧Wikiページにリダイレクトされてしまう方はお手数ですがブラウザのキャッシュを削除してみて下さい。 </span>

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130519/20130519235217.png" alt="f:id:tuto0621:20130519235217p:plain" title="f:id:tuto0621:20130519235217p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## 移行の経緯

MilkodeのホームページはずっとMediaWikiを使っていたのですが、以前別の自作ソフトのドキュメント作成にJekyllを使い結構感触が良かったので思い切って移行することにしました。

文章量が多かったため移行に2週間位かかりました。

## MediaWikiからJekyllに移行して良かったこと

- ファイルで原稿を書ける
  - Rubyを使ってフィルター処理をしたり、まとめてコピーといった一括処理が簡単です
- Markdownで書ける
  - emacsのmarkdown-modeやKobito等、対応しているエディタが多いのがいい所です
- git管理出来る
  - 編集履歴が見やすい形で残ります、差分の再利用などもやりすくなります
- ページの表示が高速
  - 事前にhtmlを生成しておくので高速です(MediaWikiのレンダリングもそんなに遅いという訳ではないですが)
- スパム攻撃を受けない
  - [以前スパム攻撃で苦労](http://ongaeshi.hatenablog.com/entry/20110906/1315324689)して最終的に自分以外は書き込み不可にすることになり、それじゃWikiの意味ないじゃん・・とは思ってました
- _layouts/layout.html と Liquidテンプレートの組み合わせで結構色々なことが出来る
  - 変数展開やちょっとした繰り返し処理が書けます
  - ヘッダやフッターはlayout.htmlに一回だけ書いてメイン記事となるxxx.mdには記事本体しか書かないように出来ます
  - 「面倒なHTMLを書かずにすぐに記事本文を書き始めたい」は自分にとってWikiを使う大きなモチベーションの一つだったのでこれで切り替える気になりました
  - とはいえ最初の一回は自分でテンプレート書く必要がありますが、もう一回書いたので次からはもっと楽に出来そうです

大勢で編集する時はやはりWikiがいいですが、主に一人で作っているサイトをJekyllで作るのはなかなかいいと思いました。

## 実際にJekyllドキュメントシステムを動かしてみる
[ongaeshi/milkode at gh-pages · GitHub](https://github.com/ongaeshi/milkode/tree/gh-pages)

お手元の環境で再現するには

```
$ git clone git://github.com/ongaeshi/milkode.git
$ cd milkode
$ git checkout -b gh-pages origin/gh-pages
```

でソースコードをチェックアウトした後

```
$ jekyll serve --watch
Configuration file: xxx
            Source: xxx
       Destination: xxx/_site
      Generating... done.
 Auto-regeneration: enabled
[2013-05-19 23:16:07] INFO  WEBrick 1.3.1
[2013-05-19 23:16:07] INFO  ruby 2.0.0 (2013-02-24) [x86_64-darwin11]
[2013-05-19 23:16:07] INFO  WEBrick::HTTPServer#start: pid=31564 port=4000
```

で、webサーバーを起動した後に http://0.0.0.0:4000 にアクセスして下さい。

jekyllをインストールしていない人は事前に 

```
$ gem install jekyll
``` 

が必要です。

Jekyllの簡易テンプレートやサンプルコードとしてお使い下さい。

## ページをカスタマイズする

- ページの内容を書き換えたい → `*.md`を編集
- ページテンプレートを書き換えたい → `layout.html
- Jekyllの設定やsite.xxxといった変数の値を書き換えたい → `_config.yml`

<span style="color: #cc0000"><b>注意</b></span>: `_config.yml`を書き換えた時は `jekyll serve --watch` を一回停止して再起動しないと変更が反映されません。(たまに忘れます・・)

## 関連記事
- [30分のチュートリアルでJekyllを理解する](http://melborne.github.io/2012/05/13/first-step-of-jekyll/)
- [リンクタイトルとURLをその場で編集→好きな形式でクリップボードにコピーしてくれる FireLink](http://ongaeshi.hatenablog.com/entry/20120906/1346896160) - こちらのページもJekyllに移行済みです

## お知らせ(宣伝ともいう)
おそらく明日辺りに[gihyo.jp](http://gihyo.jp/)にMilkodeの記事が載ります。Milkodeにおけるgroonga(主にrroonga)の使い方について書きました。

[隔週連載groonga](http://gihyo.jp/dev/clip/01/groonga)の**第四回**として掲載される予定です。

- Rubyで検索エンジンを作ってみたい
- 作るかは分からないけどその辺の技術に興味はある

ような方にとっては面白いかもしれません、お楽しみに～。
