---
Title: Ruby on Rails チュートリアルをやってみる 〜1日目〜
Category:
- ruby
- rails
- book
Date: 2014-04-26T21:18:18+09:00
URL: https://ongaeshi.hatenablog.com/entry/railstutorial-01
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815722744168
---

<span style="font-size: 150%">[目次](http://ongaeshi.hatenablog.com/entry/railstutorial)
</span>

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140426/20140426211642.png" alt="f:id:tuto0621:20140426211642p:plain" title="f:id:tuto0621:20140426211642p:plain" class="hatena-fotolife" itemprop="image"></span></p>

もうすぐ連休も近づいてきて何をやろうかなと思っていたのだけど、Railsチュートリアルをやってみることにした。今までSinatraでWebアプリを作ることが多かったので一回Railsの流儀を覚えたい。ラズベリーパイにゲーム機を載せるのとどっちがいいかな、と思っていたけど今回はこちら。

[Ruby on Rails チュートリアル: 実例を使ってRailsを学ぼう - 達人出版会](http://tatsu-zine.com/books/railstutorial)

最後までいくとマルチユーザー、認証あり、データベース付きで動くTwitterっぽいアプリケーションが出来るらしい、楽しみ。

第一章はRuby, Rails, GitHub, Heroku のセットアップ作業が主になった。(最大の難関と言ってもいいかもしれない)

## コマンドラインツールのインストール
まず XCode コマンドラインツールが古かったので更新。

[Mac での Xcode コマンド ライン ツールのインストール - RAD Studio](http://docwiki.embarcadero.com/RADStudio/XE4/ja/Mac_%E3%81%A7%E3%81%AE_Xcode_%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89_%E3%83%A9%E3%82%A4%E3%83%B3_%E3%83%84%E3%83%BC%E3%83%AB%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)

[Downloads for Apple Developers］リストで、必要な［Command Line Tools］の項目を選択。私の場合は <b>Command Line Tools (OS X Lion) for Xcode</b> を選択。右上で検索すると見つからずに無限ループするので注意。左上で絞り込み検索。

## RVMのインストール
RVMのライブラリ取得先が何故か Homebrew になっていたので MacPorts に戻す (デフォルトこっちのはずなんだけど、試した時に変えてしまったのだろうか？)

```
$ rvm autolibs macports
```

## ncursesw を削除して ncurses をインストール

RVM 経由で Ruby をインストールしようとしたら、 ncursesw という古いパッケージを消して ncurses を使えと怒られたので消す。普通に消すと怒られるので `-f` を付ける。

```
$ rvm install 2.0.0 --with-openssl-dir=$HOME/.rvm/usr
Checking requirements for osx_port.
Error! ncursesw was replaced by ncurses a long time ago, please uninstall 'ncursesw',
         for more details check http://comments.gmane.org/gmane.os.apple.macports.user/28039
Requirements installation failed with status: 1.

$ sudo port -f uninstall ncursesw
--->  The following versions of ncursesw are currently installed:
--->      ncursesw @5.7_0+darwin_10
--->      ncursesw @5.8_0 (active)
Error: port uninstall failed: Registry error: Please specify the full version as recorded in the port registry.

$ sudo port -f uninstall ncursesw @5.8_0
--->  Unable to uninstall ncursesw @5.8_0, the following ports depend on it:
--->  	ncurses @5.7_0+darwin_10
Warning: Uninstall forced.  Proceeding despite dependencies.
--->  Deactivating ncursesw @5.8_0
--->  Cleaning ncursesw
--->  Uninstalling ncursesw @5.8_0
--->  Cleaning ncursesw

$ sudo port -f uninstall ncursesw
--->  Unable to uninstall ncursesw @5.7_0+darwin_10, the following ports depend on it:
--->  	ncurses @5.7_0+darwin_10
Warning: Uninstall forced.  Proceeding despite dependencies.
--->  Uninstalling ncursesw @5.7_0+darwin_10

$ sudo port install ncurses
--->  Cleaning ncurses
--->  Scanning binaries for linking errors: 100.0%
--->  No broken files found.
```

## Rubyをインストール

コマンドラインツールとRVMのインストール周りが一番大変だった。以後は大分スムーズにいけた。

```
$ rvm install 2.0.0 --with-openssl-dir=$HOME/.rvm/usr
.
.
Install of ruby-2.0.0-p451 - #complete 
Please be aware that you just installed a ruby that requires        2 patches just to be compiled on an up to date linux system.
This may have known and unaccounted for security vulnerabilities.
Please consider upgrading to ruby-2.1.1 which will have all of the latest security patches.
Ruby was built without documentation, to build it run: rvm docs generate-ri
```

gemset を作成

```
$ rvm use 2.0.0@railstutorial_rails_4_0 --create --default
ruby-2.0.0-p451 - #gemset created /Users/ongaeshi/.rvm/gems/ruby-2.0.0-p451@railstutorial_rails_4_0
ruby-2.0.0-p451 - #generating railstutorial_rails_4_0 wrappers..........
Using /Users/ongaeshi/.rvm/gems/ruby-2.0.0-p451 with gemset railstutorial_rails_4_0
```

gem を 2.0.3 に更新 (実際はダウングレード)

```
$ gem -v
2.2.2
$ gem update --system 2.0.3
Updating rubygems-update
```

## Rails のインストール

```
$ gem install rails --version 4.0.2
Fetching: i18n-0.6.9.gem (100%)
Successfully installed i18n-0.6.9
Fetching: multi_json-1.9.2.gem (100%)
Successfully installed multi_json-1.9.2
Fetching: tzinfo-0.3.39.gem (100%)
Successfully installed tzinfo-0.3.39
Fetching: thread_safe-0.3.3.gem (100%)
Successfully installed thread_safe-0.3.3
Fetching: activesupport-4.0.2.gem (100%)
Successfully installed activesupport-4.0.2
Fetching: builder-3.1.4.gem (100%)
Successfully installed builder-3.1.4
Fetching: rack-1.5.2.gem (100%)
Successfully installed rack-1.5.2
Fetching: rack-test-0.6.2.gem (100%)
Successfully installed rack-test-0.6.2
Fetching: erubis-2.7.0.gem (100%)
Successfully installed erubis-2.7.0
Fetching: actionpack-4.0.2.gem (100%)
Successfully installed actionpack-4.0.2
Fetching: activemodel-4.0.2.gem (100%)
Successfully installed activemodel-4.0.2
Fetching: arel-4.0.2.gem (100%)
Successfully installed arel-4.0.2
Fetching: activerecord-deprecated_finders-1.0.3.gem (100%)
Successfully installed activerecord-deprecated_finders-1.0.3
Fetching: activerecord-4.0.2.gem (100%)
Successfully installed activerecord-4.0.2
Fetching: mime-types-1.25.1.gem (100%)
Successfully installed mime-types-1.25.1
Fetching: polyglot-0.3.4.gem (100%)
Successfully installed polyglot-0.3.4
Fetching: treetop-1.4.15.gem (100%)
Successfully installed treetop-1.4.15
Fetching: mail-2.5.4.gem (100%)
Successfully installed mail-2.5.4
Fetching: actionmailer-4.0.2.gem (100%)
Successfully installed actionmailer-4.0.2
Fetching: thor-0.19.1.gem (100%)
Successfully installed thor-0.19.1
Fetching: railties-4.0.2.gem (100%)
Successfully installed railties-4.0.2
Fetching: hike-1.2.3.gem (100%)
Successfully installed hike-1.2.3
Fetching: tilt-1.4.1.gem (100%)
Successfully installed tilt-1.4.1
Fetching: sprockets-2.12.1.gem (100%)
Successfully installed sprockets-2.12.1
Fetching: sprockets-rails-2.0.1.gem (100%)
Successfully installed sprockets-rails-2.0.1
Fetching: rails-4.0.2.gem (100%)
Successfully installed rails-4.0.2
26 gems installed
```

```
$ rails -v
Rails 4.0.2
```

これで準備はととのった。

## rails new

```
$ rails new first_app
$ cd first_app
```

```
app/             モデル、ビュー、コントローラ、ヘルパーなどを含む主要なアプリケーションコード
  assets         アプリケーションで使用するCSS (Cascading Style Sheet)、JavaScript ファイル、画像などのアセット
bin/             バイナリ実行可能ファイル
config/          アプリケーションの設定
db/              データベース関連のファイル
doc/             マニュアルなど、アプリケーションのドキュメント
lib/             ライブラリモジュール
  assets         ライブラリで使用する CSS (Cascading Style Sheet)、JavaScriptファイル、画像などのアセット
log/             アプリケーションのログファイル
public/          エラーページなど、一般 (Web ブラウザなど) に直接公開するデータ
script/rails     コード生成、コンソールの起動、ローカルの Web サーバーの立ち上げなどに使用する Rails スクリプト
test/            アプリケーションのテスト (3.1.2 で作成する spec/ディレクトリがあるため、現在は使用されていません)
tmp/             一時ファイル
vendor/          サードパーティのプラグインや gem など
  assets         サードパーティのプラグインや gem で使用する CSS (CascadingStyle Sheet)、JavaScript ファイル、画像などのアセット
README.rdoc      アプリケーションの簡単な説明
Rakefile         rake コマンドで使用可能なタスク
Gemfile          このアプリケーションに必要な Gem の定義ファイル
Gemfile.lock     アプリケーションのすべてのコピーが同じ gem のバージョンを使用していることを確認するために使用される gem のリスト
config.ru        Rack ミドルウェア用の設定ファイル
.gitignore       Git に無視させたいファイルを指定するためのパターン
```

## rails server

```
$ rails server
```

http://localhost:4000/ にアクセス。


<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140426/20140426211642.png" alt="f:id:tuto0621:20140426211642p:plain" title="f:id:tuto0621:20140426211642p:plain" class="hatena-fotolife" itemprop="image"></span></p>
最初のアプリ動いた！さっそく GitHub に登録。(ここも丁寧に解説してくれる)

[ongaeshi/first_app](https://github.com/ongaeshi/first_app)

## おまけ: Git Tips
たまに入る Git Tips が結構嬉しい。Qiita にまとめた。

* [Gitで間違って消したファイルやディレクトリを復活させたい](http://qiita.com/ongaeshi/items/6305e5c2fde5719c50a8)
* [Gitのファイル名をコマンド一発で変更する](http://qiita.com/ongaeshi/items/6820fd124bfcb32d872e)

## 1.4.1 Heroku のセットアップ
[Heroku Toolbelt](https://toolbelt.heroku.com/osx) をインストール。そのあとEmacsを再起動したら使えた。

動くのだけど sudo を使わないといけない。

```
$ sudo heroku create
sudo heroku create
Creating vast-retreat-6363... done, stack is cedar
http://vast-retreat-6363.herokuapp.com/ | git@heroku.com:vast-retreat-6363.git
Git remote heroku added
$ git push heroku master
$ heroku open
```

.netrc の[所有権を変更](http://stackoverflow.com/questions/10670169/permission-denied-for-heroku-create-command)することで直った。

```
$ ls -la .netrc 
-rw-------  1 root  staff  213  4 26 11:29 .netrc
$ sudo chown ongaeshi .netrc 
```

作ったものを[Herokuにデプロイ](http://railstutorial-ong.herokuapp.com/)。まだエラーが出るけどチュートリアルが進むと直るらしい。

