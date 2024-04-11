---
Title: MilkodeをWindows10 Ruby 2.4で動かす
Category:
- milkode
Date: 2018-01-29T15:43:25+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-windows-10-ruby2.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812341871323
---

[groonga-dev](https://ja.osdn.net/projects/groonga/lists/archive/dev/2018-January/thread.html#4568)に質問メール送ったらすぐに対応してくれた。

[groonga-dev,04569]

> RubyInstaller2からはPATH環境変数を使わずに独自でDLLを探すようになっているので、それに対応させないとGroongaのDLLを見つけられないんです。
>
> なので、↑のgemに
>   https://github.com/ranguba/rroonga/commit/5f37d5c0a9b28155b5d56a590243239097343c65
>
> の変更を入れてみてください。
>
> これでrequireできるようになるはずです。

そうなのかー、超勉強になった。

[groonga-dev,04570]

> なるほど…！？
>
> ありがとうございます！
> ビルドし直して更新しました。（今はWindows環境がないので試せませんが…）

やったー。

## さっそくためす


```
$ gem install /c/Users/ongaeshi/Downloads/rroonga-7.0.3-x64-mingw32.gem
Successfully installed rroonga-7.0.3-x64-mingw32
Parsing documentation for rroonga-7.0.3-x64-mingw32
Installing ri documentation for rroonga-7.0.3-x64-mingw32
Done installing documentation for rroonga after 7 seconds
1 gem installed
```

インストールには成功。

```
$ ruby -r "rroonga" -e ""
```

エラーがでなくなった！

## eventmachineでエラー

早速Milkodeをインストールして動かしてみた。しかし`milk web`が動かない。

```
$ milk web
Unable to load the EventMachine C extension; To use the pure-ruby reactor, require 'em/pure_ruby'
Unable to load the EventMachine C extension; To use the pure-ruby reactor, require 'em/pure_ruby'
C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- 2.4/rubyeventmachine (LoadError)
        from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from C:/Ruby24-x64/lib/ruby/gems/2.4.0/gems/eventmachine-1.2.5-x64-mingw32/lib/rubyeventmachine.rb:2:in `<top (required)>'
        from C:/Ruby24-x64/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:133:in `require'
```

どうもeventmachine gemの調子が悪いらしい?エラーメッセージをググってみる。

- [Ruby 2.4 Compatible? · Issue #806 · eventmachine/eventmachine](https://github.com/eventmachine/eventmachine/issues/806)

>Note that Windows binaries are not available for Ruby 2.4 yet, in that case you'll need to compile yourself, which is easy with: gem install eventmachine --platform ruby

これっぽい。eventmachineを一度アンインストールして`platform=ruby`付けて再インストール。

```
$ gem uninstall -I eventmachine
.
.
$ gem install eventmachine --platform ruby
Temporarily enhancing PATH for MSYS/MINGW...
Building native extensions.  This could take a while...
Successfully installed eventmachine-1.2.5
Parsing documentation for eventmachine-1.2.5
Installing ri documentation for eventmachine-1.2.5
Done installing documentation for eventmachine after 8 seconds
1 gem installed
```

これで`milk web`も動くようになった。

## まとめ

まとめると以下の手順でMilkodeをWindows10 Ruby2.4でインストールすることができるようになる(予定)。

```
$ gem install eventmachine --platform=ruby
$ gem install milkode
```

ただし[rroonga 7.0.3](https://rubygems.org/gems/rroonga/)以降じゃないと駄目なのでリリースされるのをもう少し待ちましょう。(groonga-dev MLには連絡済み)
