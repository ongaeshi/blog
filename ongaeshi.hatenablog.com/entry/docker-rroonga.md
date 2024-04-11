---
Title: Groongaを使ったRubyアプリを簡単に配布するためのDocker Rroongaを作った
Date: 2015-11-24T22:53:51+09:00
URL: https://ongaeshi.hatenablog.com/entry/docker-rroonga
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347146196785
---

Rroonga(GroongaをRubyから簡単に使えるライブラリ)を使ったWebアプリケーションを簡単にDockerコンテナとして配布できるようにするために、DockerHubに[groonga/rroonga](https://hub.docker.com/r/groonga/rroonga/)をおきました。[Honyomi 1.5](http://ongaeshi.hatenablog.com/entry/honyomi-1.5)ですでに使われています。

## 使い方
DockerファイルのFROMにgoonga/rroongaを指定するだけです。

```
FROM groonga/rroonga:latest

# Add your setting

```

[docker-honyomiのDockerfile](https://github.com/ongaeshi/docker-honyomi/blob/master/Dockerfile)を読むとなんとなく何をやれば良いか分かるのではないかと思います。

## Dockerコンテナを使ってRroongaチュートリアルを実行する
Dockerコンテナの使い道はこれだけではありません。[File: tutorial — rroonga - ラングバ](http://ranguba.org/rroonga/ja/file.tutorial.html)を自身の環境を汚さず安全に試すことができます。

```
$ docker run --name rroonga -it groonga/rroonga
irb(main):001:0> require 'groonga'
=> true
irb(main):002:0> Groonga::VERSION
=> [5, 0, 9, nil]
irb(main):003:0> Groonga::Context.default_options = {:encoding => :utf8}
=> {:encoding=>:utf8}
.
.
```

groonga/rroongaコンテナを引数なしで起動すると<b>自動的にirbが立ち上がります</b>。(FROMに指定している[公式Rubyコンテナ](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.2/Dockerfile)の機能です)

コンテナにはRroongaがインストール済みなのですぐにチュートリアルを試すことができます。チュートリアルを一通り実行するとRroongaの使い勝手がなんとなく分かるのではないかと思います。

## Dockerコンテナを開発環境として使う

チュートリアルの後にスクリプトを書きたくなったら共有フォルダを指定して/bin/bashで起動します。

```
$ docker run -it -v /path/to/src:/home groonga/rroonga /bin/bash
```

私はホームディレクトリを共有フォルダに指定して起動しています。([yourname]を自分のフォルダ名に置き換え)

```
$ docker run -it -v /Users/[yourname]:/home groonga/rroonga /bin/bash
```

するとコンテナからホームディレクトリにアクセスできるようになります。

```
% cd /home
% ls
Applications
Downloads
Public

% cd Documents
% ls
auto-shell-command
f-ruby-07-demo
jekyll-test2
react-0.13.2
.
.
```

ホストマシンでスクリプトを書いて実行はシェル上から行うことができます。

[grn_mini](https://github.com/ongaeshi/grn_mini)を使った簡単なテストコードを実行してみます。

```ruby
require 'grn_mini'

GrnMini::tmpdb do
  array = GrnMini::Array.new
  array << {text: "aaa", number: 1}
  array << {text: "bbb", number: 2}
  array << {text: "ccc", number: 3}
  array << {text: "aaa", number: 4}

  p array.count
  p array.select("text:aaa").count
  p array.select("text:aaa number:>1").count
end
```

先にgrn_mini gemをインストールしてからスクリプトを実行してます。いちいちsudoが要らないのもいいですね。

```
% gem install grn_mini
% cd grn_mini_test
% ruby hello.rb
4
2
1
```

## (宣伝) Groonga Meetup 2015 で発表します
11/29に開催される[Groonga Meatup 2015](https://groonga.doorkeeper.jp/events/31482)で「GroongaアプリケーションをDockerコンテナ化して配布する」というタイトルで発表する予定です。

この記事の内容や[Kitematic](http://qiita.com/ongaeshi/items/1e6fd25d3c9c27f6f376)の実演なども行う予定なので興味のある方は是非ご参加ください。

