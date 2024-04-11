---
Title: WindowsのRuby開発環境を整える
Category:
- 開発環境
Date: 2018-01-21T23:00:07+09:00
URL: https://ongaeshi.hatenablog.com/entry/install-ruby-windows
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812339732214
---

[前回](http://ongaeshi.hatenablog.com/entry/install-msys2)でmsys2のインストールに成功したので、次はWindowsのRuby開発環境を整えることにする。せっかくなので最新のRuby2.5を入れてみる。

- [RubyInstaller2でWindows環境にRuby 2.4 + Rails 5.0.2をインストールする - Qiita](https://qiita.com/jnchito/items/08b5be458134073c60e3)

## 手順

- 古いRubyが入っていたのでアンインストール
- `rubyinstaller-2.5.0-1-x64.exe`をダブルクリック
- デフォルトの外部エンコーディングをUTF-8にするオプションがあったのでONに(もうSJISのテキストはあまり無いだろう・・)
[f:id:tuto0621:20180121230051p:plain]
- インストール終了後に開発キットのインストール画面に移動
[f:id:tuto0621:20180121230221p:plain]
- msys2のインストールはすでに済んでいるので`3`を選択
[f:id:tuto0621:20180121230201p:plain]

無事インストールされているか確認する。

```
$ ruby --version
ruby 2.5.0p0 (2017-12-25 revision 61468) [x64-mingw32]
```

## Gemのインストール

早速Milkodeをインストール。

```
$ gem install milkode
ERROR:  Error installing milkode:
        The last version of rroonga (>= 1.1.0) to support your Ruby & RubyGems was 7.0.2. Try installing it with `gem install rroonga -v 7.0.2` and then running the current command again
        rroonga requires Ruby version < 2.5, >= 2.1. The current ruby version is 2.5.0.
Successfully installed highline-1.7.10
Successfully installed termcolor-1.2.1
Successfully installed pkg-config-1.2.9
Successfully installed gqtp-1.0.6
Successfully installed groonga-command-1.3.4
Successfully installed json-stream-0.2.1
Successfully installed groonga-command-parser-1.1.2
Successfully installed hashie-3.5.7
Successfully installed groonga-client-0.5.8
Successfully installed io-like-0.3.0
Successfully installed archive-zip-0.10.0

```

rrooongaが2.5だとインストールできない？なんですとー。試しにnokogiriもインストールしてみたら同じエラー。まだちょっと早かったかな・・。

追記: [公式ページ](https://rubyinstaller.org/downloads/)読んだら2.5はgemの問題があるからまだ使うなって書いてあるね・・。

```
$ gem install nokogiri
ERROR:  Error installing nokogiri:
        The last version of nokogiri (>= 0) to support your Ruby & RubyGems was 1.8.1. Try installing it with `gem install nokogiri -v 1.8.1`
        nokogiri requires Ruby version < 2.5, >= 2.2. The current ruby version is 2.5.0.
Successfully installed mini_portile2-2.3.0

```

## Ruby2.4をインストール

改めて`rubyinstaller-2.4.3-1-x64.exe`をインストール。msys2開発キットのインストールはすでに終わっていたのですぐにインストールが終わった。

```
$ gem install milkode
$ gem install nokogiri
```

どちらも無事インストールできた！しかしMilkodeを実行しようとすると動かないエラーが。

```
$ milk init
C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- groonga.so (LoadError)
```

groonga.soが見つからないとのこと。試しにfindしてみる。

```
ongaeshi@DESKTOP MSYS /c/Ruby24-x64
$ find . -name "groonga.so"
./lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/2.1/groonga.so
./lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/2.2/groonga.so
./lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/2.3/groonga.so
```

`2.1`、`2.2`、`2.3`は用意されているけど、2.4はもしかしてない？

明日もう少し調べてみよう。
