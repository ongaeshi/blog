---
Title: Rubyから外部コマンドを実行するときはsystemやOpen3に可変長引数で渡すのが便利
Category:
- ruby
Date: 2015-11-06T01:37:09+09:00
URL: https://ongaeshi.hatenablog.com/entry/ruby-shellwords2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415127071505
---

[Rubyから外部コマンドを実行するときはShellwordsモジュールが便利](http://ongaeshi.hatenablog.com/entry/ruby-shellwords)の続きです。ブコメで教えてもらって基本的な使い方だったら、`Shellwords.escape`を使わずに済むことが分かりました。

[http://b.hatena.ne.jp/entry/270516861/comment/sora_h:embed:cite]

## systemの場合

```ruby
system("ls #{ARGV[0]}")                         # エスケープされていない、危険
system("ls #{Shellwords.escape(ARGV[0])}")　　　# エスケープされる
system("ls", ARGV[0])                          # エスケープされる(そして短い)
system(*["ls", ARGV[0]])                       # 可変長引数なので配列も渡せる
```

配列を渡すときに先頭にアスタリスクをつけるのは[Ruby: 可変長引数と配列 - 日々の報告書](http://d.hatena.ne.jp/stakizawa/20100126/t1)。

## Open3.capture3

systemと基本同じです。

```ruby
o, e, s = Open3.capture3("ls #{ARGV[0]}")                         # エスケープされていない、危険
o, e, s = Open3.capture3("ls #{Shellwords.escape(ARGV[0])}")　　　# エスケープされる
o, e, s = Open3.capture3("ls", ARGV[0])                          # エスケープされる(そして短い)
o, e, s = Open3.capture3(*["ls", ARGV[0]])                       # 可変長引数なので配列も渡せる
```

## おまけ: systemやcapture3の内部ではShellwords.escapeが呼ばれているのか？
やっていることは似たようなことなので実は内部では関数が共有されているかな？と思い調べてみました・・。

**呼ばれていませんでした**(多分) m(__)m。 `Open3.capture3`のソースを読むと引数は`Kernel.spawn`というメソッドに渡されます。Kernel.spawnの実体はC言語の`rb_spawn`という関数のようです。(rb_spawnの中でエスケープが行われているか最終的なプロセス実行のAPIがエスケープしなくてよいのだと予想)

[ruby/lib/open3.rb](https://github.com/ruby/ruby/blob/832c74f428db6c5bd6e575e1f6ea7fe0891c84d2/lib/open3.rb)

```
Open3.capture3
  Open3.popen3
    Open3.popen_run
      Kernel.spawn (Process.spawn)
--- ここからCへ潜る ---
rb_spawn
  rb_spawn_internal
.
.
```

一方Shellwordsモジュールは完全にRubyで書かれた独立したライブラリのようです。`Shellwords.escape`関数はコメントを抜くとたった6行で表現されていて感動します。

[ruby/lib/shellwords.rb](https://github.com/ruby/ruby/blob/832c74f428db6c5bd6e575e1f6ea7fe0891c84d2/lib/shellwords.rb#L123)

```ruby
  def shellescape(str)
    str = str.to_s

    # An empty argument will be skipped, so return empty quotes.
    return "''" if str.empty?

    str = str.dup

    # Treat multibyte characters as is.  It is the caller's responsibility
    # to encode the string in the right encoding for the shell
    # environment.
    str.gsub!(/([^A-Za-z0-9_\-.,:\/@\n])/, "\\\\\\1")

    # A LF cannot be escaped with a backslash because a backslash + LF
    # combo is regarded as a line continuation and simply ignored.
    str.gsub!(/\n/, "'\n'")

    return str
  end

  .
  .

  class << self
    alias escape shellescape
  end
```

さて、ソースを読んでみると`Shellwords.escape`で渡す方式はわざわざRuby側でエスケープした文字列に変換してから1つ文字列に連結してspawnに渡しています。

それに対して可変長引数で渡す方式はプログラムから渡された未処理の引数が直接spawnに渡され、後はrb_spawn内で処理されます。パフォーマンス的にも可変長引数に渡す方式のほうが効率が良さそうです(第1引数をコマンドとして実行するのでシェルも通さないし)。

## まとめ
- コマンドを実行するだけだったらsystemやcapture3の可変長引数に渡すのが簡単で安全(パフォーマンス的にもお得そう)
- コマンド実行以外の目的でシェルに安全に渡せるエスケープされた文字列が欲しかったりコマンド引数を分解したいようなときはShellwordsモジュールを使う

勉強になりました。
