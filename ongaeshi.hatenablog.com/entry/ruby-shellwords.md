---
Title: Rubyから外部コマンドを実行するときはShellwordsモジュールが便利
Category:
- ruby
- shell
- unix
Date: 2015-11-03T11:00:41+09:00
URL: https://ongaeshi.hatenablog.com/entry/ruby-shellwords
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415126755876
---

<span style="font-size: 150%">[続き](http://ongaeshi.hatenablog.com/entry/ruby-shellwords2)を書きました。</span>

少し前にakrさんからもらった[パッチ](https://github.com/ongaeshi/milkode/pull/70)で知った **Shellwords** というモジュールが便利だったので紹介します。
Rubyスクリプトからシェルにコマンドを投げ込むときに必要なことを代わりにやってくれるいいやつです。

[module Shellwords (Ruby 2.2.0)](http://docs.ruby-lang.org/ja/2.2.0/class/Shellwords.html)

## インストール
標準添付なのでrequireすればすぐに使えます。

```ruby
require 'shellwords'
```
## 問題
コマンドから引数を受け取ってlsするスクリプトを考えてみます。

```ruby
require 'shellwords'

system("ls #{ARGV[0]}")
```

だいたいのケースはこれでいいのですが、引数に空白入りのファイルが渡されたときなどに問題が起きます。

```
$ ruby ls.rb "foo bar.txt"         # foo\ bar.txt でも同様
ls: bar.txt: No such file or directory
ls: foo: No such file or directory
```

シェル→Ruby→シェルの過程でシェルのメタキャラのエスケープを考慮せずに文字列が渡されてしまうのが原因です。

```text
ruby ls.rb "foo bar.txt"      # シェル
↓
["foo bar.txt"]               # ARGV
↓
system("ls #{ARGV[0]}")       # スクリプト
↓
system("ls foo bar.txt")      # 展開されて引数が2つ渡されたことになってしまう・・
```

## 解決方法
シェルに引数として渡したいものを[Shellwords.escape](http://docs.ruby-lang.org/ja/2.2.0/class/Shellwords.html#S_ESCAPE)で囲みます。

```ruby
require 'shellwords'

system("ls #{Shellwords.escape(ARGV[0])}")
```

すると文字列をBourneシェルのコマンドライン中で安全に使えるようにエスケープしてくれます。

```
irb(main):013:0> Shellwords.escape("foo bar.txt")
=> "foo\\ bar.txt"
irb(main):014:0> Shellwords.escape("a+b+c bar.txt")
=> "a\\+b\\+c\\ bar.txt"
```

これで空白入りのファイルを渡してもうまく動くようになりました。

```
$ ruby ls.rb "foo bar.txt"
foo bar.txt
```

外からはRubyスクリプトとして振舞っているけれど、実際には内部で[system](http://docs.ruby-lang.org/ja/2.2.0/method/Open3/m/capture3.html)や[Open3.capture3](http://docs.ruby-lang.org/ja/2.2.0/method/Open3/m/capture3.html)を呼び出しているようなスクリプトを書くときはとりあえずShellwords.escapeで囲んでおくと安全に動きます。

## 応用
Shellwordsにはそれ以外にも様々な便利な機能があります。

例えば[こちら](https://github.com/ongaeshi/milkode/pull/70/files)で使われている`Shellwords.split`はシェルのメタ文字を考慮し、文字列を`split(" ")`するよりもうまく配列に分割してくれます。

```
irb(main):018:0> Shellwords.split("ls -la foo\\ bar.txt")
=> ["ls", "-la", "foo bar.txt"]
```

リンク先では分割した配列をさらに配列に積んでまとめて[Open3.pipeline](http://docs.ruby-lang.org/ja/2.2.0/method/Open3/m/pipeline.html)に渡しています。ShellwordsとOpen3モジュールを組み合わせることで途中に、xargを挟むような複雑なシェルコマンドもRuby上で安全に実行することができます。







