---
Title: Milkode1.3リリース - GitHubボタンを追加、キーワードのマッチ箇所をハイライト
Category:
- ruby
- groonga
- milkode
Date: 2013-11-05T10:58:40+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815711828063
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131102/20131102222613.png" alt="f:id:tuto0621:20131102222613p:plain" title="f:id:tuto0621:20131102222613p:plain" class="hatena-fotolife" itemprop="image"></span></p>

- GitHubボタンの追加
- キーワードのマッチ箇所をハイライト

最新のMilkodeを試してみたい時は[mrubysearch](http://mrubysearch.ongaeshi.me/)をどうぞ。

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.3.0)

## GitHubボタンの追加
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131102/20131102222622.png" alt="f:id:tuto0621:20131102222622p:plain" title="f:id:tuto0621:20131102222622p:plain" class="hatena-fotolife" itemprop="image"></span></p>

GitHubとMilkodeの連携を容易にするものです。GitHubからクローンしたレポジトリに付けられます。
※ 古いMilkodeを使っている人はバージョンアップ後に `milk update --all` で設定されます。

ボタンをクリックすると関連ページへジャンプすることが出来ます。[ngx_mruby](http://mrubysearch.ongaeshi.me/home/ngx_mruby)と[ngx_mruby/src](http://mrubysearch.ongaeshi.me/home/ngx_mruby/src) でそれぞれボタンを押してもらうと分かりますが、<b>各自対応したソースコード位置にジャンプ</b>します。GitHubボタンを押す事で、検索で見つけたパッケージにそのまま[スター](https://help.github.com/articles/stars)を付けたり、[README.md](http://mrubysearch.ongaeshi.me/home/ngx_mruby/README.md)を整形された状態で読むことが出来ます。

## キーワードのマッチ箇所をハイライト
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131102/20131102222637.png" alt="f:id:tuto0621:20131102222637p:plain" title="f:id:tuto0621:20131102222637p:plain" class="hatena-fotolife" itemprop="image"></span></p>


今までは行全体がハイライトされていましたが、マッチ箇所のみがハイライトされるようになりました。

地味な修正ですが見やすくなります。

## 全文検索エンジンGroongaを囲む夕べ 4
11/29(金)の[Groongaを囲む夕べ 4](http://atnd.org/events/43461)で今年も発表します。

- Milkode2013年の歩み（rroongaを使ったソースコード検索エンジン）

というタイトルで、今年追加した機能の総括と来年のもくろみを話す予定です。よかったら聞いてやって下さい。

## リリースノート
* milk web
  * Highlight keywords
  * Add github button  if package is github repository
  * Touch update time if exist update file only
  * Add test case

* milk
  * GitHub repository check in 'milk update'
  * Improve to not stop the package directory even if there is no

## 関連記事
- [Milkode 1.2 リリース : ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート](http://ongaeshi.hatenablog.com/entry/milkode-1.2)
- [Milkode 1.1 リリース : 待望の相対URLに対応、gmilkの高速化 ](http://ongaeshi.hatenablog.com/entry/milkode-1.1)
- [Rubyで特定のコマンドが存在するかを調べる方法](http://ongaeshi.hatenablog.com/entry/2013/06/18/144256)
- [Milkodeをgrepと組み合わせたら検索速度が40倍速くなった](http://ongaeshi.hatenablog.com/entry/milkode-and-grep)
- [行指向のソースコード検索エンジンMilkode1.0.0リリース!](http://ongaeshi.hatenablog.com/entry/milkode-1.0.0)



