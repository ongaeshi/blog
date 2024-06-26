---
Title: Dockerは個人ユーザーに何をもたらすのか
Category:
- docker
- ruby
- sinatra
- diary
Date: 2015-07-18T13:05:59+09:00
URL: https://ongaeshi.hatenablog.com/entry/docker-bring-what-to-individual-users
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450102048235
---

Qiitaに書きました。

[GUIでDockerがサクサク使えるKitematicが便利 - Qiita](http://qiita.com/ongaeshi/items/1e6fd25d3c9c27f6f376)

Kitematic便利。Dockerの敷居がまた一段下がったので是非使ってみるといいです。

ここではコラム的にDockerを自分のマシンにインストールするとどのようなメリットがあるのか？個人ユーザーに何をもたらすのかについて雑多に考えてみます。

## 人の作ったアプリを試すのに便利

KitematicのようなGUIツールを使うことでもっとも便利になるところです。

[IRuby notebook](http://qiita.com/ongaeshi/items/1e6fd25d3c9c27f6f376#iruby-notebook-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%82%92%E5%8B%95%E3%81%8B%E3%81%99)のような複雑なアプリケーションを動かすときもホスト環境を汚さずに試すことが出来ます。IRuby notebookとIPython notebook、どっちがいいのかなぁと比較するのもとても簡単です。

DockerHubやGitHubからよいDockerfileのコンテナを探す能力が重要になりそうです。

## デプロイのテスト環境を構築するのに便利

Dockerコンテナを組み合わせてデプロイすることで本番環境と同じような構成で実験することが出来ます。環境の移行やバックアップも簡単です。

ここはすでにたくさんの人が言及している部分なのでこの位にしておきます。

## 開発環境として使うにはもう少し工夫がいりそう

さらにもう一歩すすめて、OSXやWindowsといったホストOSの環境を汚さずに新しい開発環境を構築できるかな、と思ったのですがいくつかクリアするべき課題が残っているように感じます。

### コンテナの再構築に時間がかかる

<span style="font-size: 150%"><span style="color: #d32f2f"><b>解決しました</b></span></span>`docker run`の`-v`オプションを使うとよいです → [docker-sinatra-devを書いた](http://ongaeshi.hatenablog.com/entry/docker-sinatra-dev)

どうせならデプロイ環境と同じ環境で開発出来たらいいなぁと思い、

- コンテナ1: MongoDB
- コンテナ2: Sinatraアプリ (コンテナ1をストレージとして使う)

で動くアプリを作ってみることにしました。

まずコンテナ1を構築します。MongoDBをインストールするDockerfileを書いて、起動・・、あっさりMongoDBのコンテナが立ち上がりました、素晴らしい。

次にコンテナ2を立ち上げます。とりあえず小さなapp.rbを読むだけのSinatraコンテナを作って、起動・・、MongoDBとの通信も出来ているようです。素晴らしい。

で、次です。目的のアプリを作るためにapp.rbを書き換えていきます。書き換え、コンテナ再構築・・、書き換え、コンテナ再構築・・・、書き換え、コンテナ再構築・・・・、エラー・・・・・。コンテナ再構築・・・・・・。残念ながらファイルを少しずつ書き換えながらその都度コンテナを再構築するのは時間がかかりすぎで効率が悪いように感じました。

少なくともコンテナ2に相当する部分(Rubyスクリプトを書き換えて何度も再起動する部分)はホストOS上に構築するのがよさそうです。

### 考察1: 開発用のコンテナを作ったらどうか？

とはいうものの、開発はホストOSでやらなきゃ駄目だよね、という結論だと面白くないのでもう少し考えてみます。

コンテナ2も本番環境用と同じものが使えるといいなぁと思っていましたが、そこは諦めて<b>Sinatraアプリ開発用コンテナ</b>というのを専用に作ることにします。

- コンテナ1: MonogoDB (↑と同じやつ)
- コンテナ2: Sinatraアプリ開発用コンテナ

通常アプリケーションコンテナのDockerfileは最後に`CMD`でapp.rbなどをキックして終了しますが、開発用コンテナはキックしません。その代わりに開発に必要なgemのインストールを済ませた状態でコンテナを立ち上げ、後はsshでログインして開発を行います。

1人で使うのであればホストOSに開発環境をインストールするのと変わらないじゃんとなりますがDockerfileの素晴らしいところはその再現性です。例えば<b>Railsアプリ開発用コンテナ</b>というのを作りpryやruby-buildといった便利なツールがインストールされた状態ですぐにRailsアプリ開発が始められるDockerfileが配布、共有されたらどうでしょうか？

初心者が最初につまずく開発環境セットアップが確実に成功し、すぐにアプリケーションの開発をはじめることができます。これは便利かもしれません。

### 考察2: 開発用コンテナのエディタ

ここまで都合のよいように書いてきましたが1つ問題が残っています。Dockerコンテナ内のファイルをどうやって編集するか、ということです。

- 案1. ホスト側からコンテナ内のファイルを編集する

Emacsには[Tramp And Docker](http://www.emacswiki.org/emacs/TrampAndDocker)や[docker-tramp.el](https://github.com/emacs-pe/docker-tramp.el)のようにホスト側からコンテナ内のDockerファイルを参照する仕組みがあるようです。

ホストOSから透過的にコンテナ内のファイルをFinderから閲覧したり、自前のエディタで編集出来るようになると好きなエディタが使えるようになるので大分便利なのですが。

- 案2. コンテナ内にエディタをインストールしてターミナル経由で編集する

EmacsやVimはターミナルモードで起動出来るので、開発用コンテナにエディタをインストール、.emacs.d/などの設定ファイルをGitHubから取ってくる、という部分を自動化すればすぐにターミナル経由でいつもの使い勝手で使えるようになります。

あれ？Emacs最強じゃね？意図していなかったのに謎の結論になりました。

まぁそれは冗談ですが、開発用コンテナというものが普及するとしたら、またターミナル経由で使えるエディタが復権するきっかけになるのかもしれません。




