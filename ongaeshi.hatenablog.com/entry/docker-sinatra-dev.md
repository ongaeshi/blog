---
Title: Dockerでホスト環境を汚さずにSinatraの開発環境を構築できるdocker-sinatra-devを書いた
Category:
- docker
- ruby
- sinatra
Date: 2015-07-19T16:28:47+09:00
URL: https://ongaeshi.hatenablog.com/entry/docker-sinatra-dev
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450102156931
---

[昨日の記事](http://ongaeshi.hatenablog.com/entry/docker-bring-what-to-individual-users)がそこそこバズったのでコメントなどで色々教えてもらいました。

調べた結果として`docker run`の`-v`オプションを使えばホストOSのファイル空間をコンテナOSに渡せることが分かりました。これを使えばファイルの編集はホストOSで行いアプリケーションの実行はコンテナ側で行うことができそうです。

さっそくシンプルなSinatra開発環境を作ってみました。

## docker-sinatra-dev

[ongaeshi/docker-sinatra-dev](https://github.com/ongaeshi/docker-sinatra-dev)

```
$ git clone https://github.com/ongaeshi/docker-sinatra-dev.git
$ cd docker-sinatra-dev
```

rakeコマンド一発でイメージの作成、コンテナの作成、実行を行ってくれます。2回目以降はイメージの作成は省略されます。

```
$ rake
docker build -t sinatra-dev .
Sending build context to Docker daemon 98.82 kB
Sending build context to Docker daemon 
Step 0 : FROM ruby:2.2.2-onbuild
# Executing 4 build triggers
Trigger 0, COPY Gemfile /usr/src/app/
.
.
docker run -itP --rm -v "/Users/ongaeshi/Documents/docker-sinatra-dev":/usr/src/app -w /usr/src/app sinatra-dev ruby app.rb -o 0.0.0.0
[2015-07-19 06:40:44] INFO  WEBrick 1.3.1
[2015-07-19 06:40:44] INFO  ruby 2.2.2 (2015-04-13) [x86_64-linux]
== Sinatra (v1.4.6) has taken the stage on 4567 for development with backup from WEBrick
[2015-07-19 06:40:44] INFO  WEBrick::HTTPServer#start: pid=1 port=4567
```

バインドされたURLをブラウザで開くとすでに[小さなSinatraアプリ](http://www.sinatrarb.com/intro-ja.html)が起動しています。

[f:id:tuto0621:20150719155652p:plain]

URLは[Kitematic](http://qiita.com/ongaeshi/items/1e6fd25d3c9c27f6f376)を使ってればGUIから簡単に確認することが出来ます。

[f:id:tuto0621:20150719155651p:plain]

ホストOSのエディタでapp.rbを編集し、

```
$ emacs app.rb
```

```diff
require 'sinatra'
require 'sinatra/reloader' if development?

get '/' do
-  'Hello world!'
+  'Hello world!!!!!!'
end
```

リロードすればアプリも更新されます。素晴らしいですね。

[f:id:tuto0621:20150719155650p:plain]

## 考察

この仕組みを使うとホストOSにインストールするものはエディタを中心とした最低限のものにとどめることができます。

例えば、大規模なオープンソースの開発環境を自分のホストOSに構築するのは少し躊躇してしまいますが(自分が実際にどれだけ貢献できるのかやってみるまで分からないけど、最初の環境構築に結構時間がかかる)、Dockerコンテナとして開発環境が提供されていればコンテナをダウンロードして試してみて、よさそうだったら継続(必要ならホストOSに環境構築してもよい)、駄目だったらイメージとコンテナを消せばよいのでずいぶん敷居が下がりそうです。

タグファイルなどをあらかじめ作成しておくのもよさそうですね。

## 応用

- 同じ仕組みを使って任意のバージョンのRuby上でコマンドを実行することも出来ます
  - [Dockerを使ってRubyスクリプトを任意のバージョンでワンライナーで実行する - Qiita](http://qiita.com/ongaeshi/items/94d443ffd333477abe81)
- Rails版は以下の記事が参考になりそうです
  - [Running a Rails Development Environment in Docker](http://blog.codeship.com/running-rails-development-environment-docker/)
