---
Title: 150行で書ける全文検索エンジン
Category:
- ruby
- groonga
- rroonga
- web
- programming
Date: 2014-01-08T10:21:33+09:00
URL: https://ongaeshi.hatenablog.com/entry/full-text-search-150
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815716011312
---

[Rubyで簡単に全文検索エンジンが作れるGrnMiniを作った](http://ongaeshi.hatenablog.com/entry/create-grn-mini)の続きです。

GrnMiniを使って小さな検索エンジンを書いてみました。

全<b>1ファイル</b>、<b>154行</b>です。検索、マッチ個所の表示(スニペット)、ページネーション、ファイル本体の表示など検索エンジンに必要な一通りの機能が入っています。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140108/20140108005433.png" alt="f:id:tuto0621:20140108005433p:plain" title="f:id:tuto0621:20140108005433p:plain" class="hatena-fotolife" itemprop="image"></span></p>

↑はあんスマさんの[青空文庫のアーカイブ](http://android-smart.com/2012/09/aozora.html)をインデックス化して検索している所です。

## インストール
grn_miniをインストールします。

```
$ gem install grn_mini
```

sinatraが入っていない人はそちらもインストールします。

```
$ gem install sinatra
```

[grn_mini/samplemini-directory-search.rb](https://github.com/ongaeshi/grn_mini_samples/blob/master/mini-directory-search.rb)を適当な場所にダウンロードします。[raw](https://raw.github.com/ongaeshi/grn_mini_samples/master/mini-directory-search.rb)からコピーするのが簡単です。

もしくは`grn_mini`をgitチェックアウトしてもよいです。

```
$ cd ~/Documents
$ git clone https://github.com/ongaeshi/grn_mini.git
```

以後はチェックアウトした`~/Documents/grn_mini/sample/mini-directory-search.rb`を使って説明します。

## 使い方

スクリプトを実行した場所<b>以下全てのファイルを検索対象とする</b>ので、検索インデックスを作りたいディレクトリに移動します。

ファイル数が多いと時間がかかるので、最初は小さめのディレクトリで実験するのがおすすめです。

```
$ cd ~/Documents/foo
```

`mini-directory-search.rb`を実行します。データベースが生成された後(最初の一回だけです。)、webアプリが立ち上がります。

```
$ ruby ~/Documents/grn_mini/sample/mini-directory-search.rb 
Create database ..
Input complete : 15 files
== Sinatra/1.4.2 has taken the stage on 4567 for development with backup from Thin
>> Thin web server (v1.5.1 codename Straight Razor)
>> Maximum connections set to 1024
>> Listening on localhost:4567, CTRL+C to stop
```

`http://localhost:4567/`をブラウザで開いて以下のような画面が出たら成功です。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140108/20140108005506.png" alt="f:id:tuto0621:20140108005506p:plain" title="f:id:tuto0621:20140108005506p:plain" class="hatena-fotolife" itemprop="image"></span></p>

以上でインストールは完了です。思うがままに検索して下さい。検索コマンドは[8.10.1. クエリー構文](http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html)をどうぞ。カラム名はソースコードをどうぞ。

## 生成した検索用データの削除
スクリプトを実行した位置に作られる`mini-directory-search.db*`という名前のファイルを全て消して下さい。

```
$ cd ~/Documents/foo
$ rm mini-directory-search.db*
```

