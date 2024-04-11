---
Title: PictRuby 0.5 リリース - チャットボット、eval、irb
Category:
- rubypico
- ruby
- mruby
- ios
Date: 2016-03-31T01:27:17+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-0.5
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792369186978
---

PictRuby 0.5 をリリースしました。 合わせて起動画面とアイコンをリニューアルしました。

[f:id:tuto0621:20160331012430p:plain:w346]

- チャットボットが書けるように
- evalが使えるように
- irbが使えるように (サンプルの10_irb.rb)

## チャットボットが書けるように
`Chat`という名前のクラスを定義すると対話型のインターフェースを構築することができるようになりました。以下はシンプルなチャットボットの例です。

[f:id:tuto0621:20160331012449g:plain]

```ruby
class Chat
  def initialize
    @num = 0
  end

  def welcome
    "Hello, World!"
  end

  def call(input)
    case input
    when "name", "Name"
      "My name is Rubo"
    when "Rubo"
      "Yes, my load."
    when "help", "Help"
      <<EOF
Echo your message

name: teach my name
halt: stop this program
EOF
    when "halt", "Halt"
      aaaa
    else
      @num += 1
      "#{@num}: #{input}"
    end
  end
end
```

一番大切なのは`call(input)`メソッドです。引数のinputにはユーザーが入力したテキストが渡されます。戻り値をレスポンスとしてbotがしゃべります。(inputをそのまま返すとエコーボットになります)

`welcome`というメソッド(optional)が定義されているとbot起動時に発言してくれます。

内部状態はChatクラスのインスタンス変数として保持してください。

## evalが使えるように
`mruby-eval`を組み込んだので`eval`、`instance_eval`が使えるようになりました。

## irbが使えるように
サンプルの`10_irb.rb`で待望のirbが使えるようになりました。これはチャットボットと`eval`の実践的なサンプルにもなっています。

[f:id:tuto0621:20160331012542p:plain:w346]

```ruby
# # 10_irb
#
# ## Description
# Interactive Ruby Shell (REPL).

class Chat
  def welcome
    <<EOS
irb - Interactive Ruby Shell
EOS
  end

  def call(input)
    begin
      eval(input)
    rescue Exception => e
      e.message
    end
  end
end
```

## 過去の記事
- [iOSのRubyプログラミング環境 PictRuby 0.4 - p, puts が使えるように](http://ongaeshi.hatenablog.com/entry/pictruby-0.4)


