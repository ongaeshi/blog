---
Title: RrroongaがRubyInstaller 2.4で動かない問題を調査
Category:
- 開発環境
Date: 2018-01-22T22:43:37+09:00
URL: https://ongaeshi.hatenablog.com/entry/rroonga-rubyinstaller-2.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812340013630
---

## rroonga 7.0.2 x64-mingw32

[Downloads](https://rubyinstaller.org/downloads/)から`rubyinstaller-2.4.3-1-x64.exe`をダウンロードしてインストール。msys2の開発キットもインストール済みなのでバイナリgemもインストール可能な状態。`gem install nokogiri`にも成功している。

```
$ ruby -v
ruby 2.4.3p205 (2017-12-14 revision 61247) [x64-mingw32]
```

rroongaのインストールには成功する。

```
$ gem install rroonga
$ gem list
rroonga (7.0.2 x64-mingw32)
```

しかし実行するとgroonga.soが見つからないといわれる。

```
$ ruby -r "rroonga" -e ""
C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- groonga.so (LoadError)
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/groonga.rb:46:in `rescue in <top (required)>'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/groonga.rb:42:in `<top (required)>'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/rroonga.rb:16:in `<top (required)>'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:133:in `require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:133:in `rescue in require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:39:in `require'
```

lib以下を見ると`2.4`が存在しない。

```
$ ls /c/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-7.0.2-x64-mingw32/lib/
2.1  2.2  2.3  groonga  groonga.rb  rroonga.rb
```

## rroonga-6.1.3-x64-mingw32

試しに一つ前のメジャーバージョンに戻る。

```
$ gem install rroonga -v 6.1.3 --platform x64-mingw32
$ gem uninstall rroonga -v 7.0.2
```

しかし同様のエラー。ただし`lib/2.4`が存在していた。

```
$ ruby -r "rroonga" -e ""
C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- groonga.so (LoadError)
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-6.1.3-x64-mingw32/lib/groonga.rb:46:in `rescue in <top (required)>'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-6.1.3-x64-mingw32/lib/groonga.rb:42:in `<top (required)>'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
   from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/rroonga-6.1.3-x64-mingw32/lib/rroonga.rb:16:in `<top (required)>'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:133:in `require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:133:in `rescue in require'
   from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:39:in `require'
```

ここまでの情報をもとにgroonga-dev MLに質問してみよう。
