---
Title: ' Milkode 1.1 リリース : 待望の相対URLに対応、gmilkの高速化'
Category:
- ruby
- milkode
- groonga
Date: 2013-06-26T17:01:20+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318755060230
---

- 相対URLに対応(http://example.com/suburl/ にデプロイ可能に)
- gmilkの検索速度を高速化

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.1.0)

## 相対URLに対応
今までは

```
http://example.com/
http://milkode.example.com
```

のように専用URLが無いとデプロイ出来なかったのですが、 

```
http://example.com/milkode
```

にも設置出来るようになりました。

### Apache+Passengerでの設定方法
1. 事前に[ApacheやPassengerの設定は済んでいる](http://shimada-k.hateblo.jp/entry/2012/12/08/140935)ものとします
1. `http://example.com/milkode`に設置したい
1. `/home/example/web/html` がドキュメントルート
1. Miilkodeのアプリケーションフォルダは`/home/example/web/milkode`
1. 書き換える`httpd.conf`は以下

```
<VirtualHost *:80>
  ServerName example.com
  DocumentRoot /home/example/web/html
</VirtualHost>
```

まず**RackBaseURI**を追記します。

```:httpd.conf
<VirtualHost *:80>
  ServerName example.com
  DocumentRoot /home/example/web/html
+  RackBaseURI  /milkode
</VirtualHost>
```

次に`/home/example/web/milkode`を作成、合わせてpublicフォルダを掘る。

```
$ mkdir /home/example/web/milkode
$ cd /home/example/web/milkode
$ mkdir public
```

次に`/home/example/web/milkode/config.ru`を作成する。

```ruby
require 'milkode/cdweb/app'
run Sinatra::Application
```

publicフォルダへweb/htmlからmilkodeという名前でシンボリックリンクを貼る。

```
$ cd /home/example/web/html
$ ln -s /home/example/web/milkode/public milkode
```

これで準備終わり。あとはhttpdを再起動すればOKです。

```
$ sudo /etc/init.d/httpd restart
```

この辺りは普通のRackアプリやSinatraアプリと変わらないので他にも色々な方法があると思います。あくまで一例です。

## gmilkの検索速度を高速化
ファイル数が多い時にgrepを使って検索速度の高速化を行いました。

詳しくは[こちら](http://ongaeshi.hatenablog.com/entry/milkode-and-grep)をどうぞ。

## リリースノート
* milk web
  * Support relative URL (e.g. http://example.com/milkode)
  * Change milkode.css -> milkode.scss (For url_for)
* milk
  * 'milk files' add --all option
* gmilk
  * Add exist_command?('cat')
  * Change how to call exist_command?
* etc
  * Add sinatra-url-for


## 関連記事
- [Rubyで特定のコマンドが存在するかを調べる方法](http://ongaeshi.hatenablog.com/entry/2013/06/18/144256)
- [Milkodeをgrepと組み合わせたら検索速度が40倍速くなった](http://ongaeshi.hatenablog.com/entry/milkode-and-grep)
- [行指向のソースコード検索エンジンMilkode1.0.0リリース!](http://ongaeshi.hatenablog.com/entry/milkode-1.0.0)
- [gihyo.jpに記事を書いた時の感想](http://ongaeshi.hatenablog.com/entry/write-article-on-gihyojp)
- [0.9.9.9(実質1.0.0.rc1) - Ruby2.0対応とか](http://ongaeshi.hatenablog.com/entry/2013/05/08/170228)



