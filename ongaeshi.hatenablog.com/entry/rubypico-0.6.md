---
Title: 小さな端末でも楽しくプログラミングできる RubyPico(旧PictRuby) 0.6 がリリース
Category:
- rubypico
Date: 2016-05-10T22:27:45+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.6
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653812171395230317
---

PictRubyは**RubyPico**という名前に変わりました！対象を画像に限らず「PC以外のスマートフォンやタブレットなど小さな端末で動くRuby」という印象にしたかったためです。

アンケートではSmartRubyが有力でしたがその後にいただいたPicoRubyという名前も評判がよく、最終的にひっくり返してRubyPicoにしました。

[https://twitter.com/ongaeshi/status/721599464482340864:embed]

0.6では待望の正規表現がサポートされました。

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## 正規表現
正規表現が書けるようになりました。`/.../`リテラル以外にも`String#match`や`String#gsub`のような`Regexp`を渡せるメソッドも使えるようになります。

```ruby
def main
  url = Popup.input "url?"
  title = Browser.get url
  m = title.match /<title>(.*?)<\/title>/
  puts m[1] if m
end
```

## Chat#timer
Chatクラスにtimerというメソッドを定義すると(ユーザーの入力を待たずに)一定間隔でbotがメッセージをしゃべることができます。

以下は3秒おきに"Hi"を出力するボットプログラムです。

nilを返したときは何も出力しません。

```ruby
class Chat
  def initialize
    @prev = Time.now    
  end
  
  def timer
    t = Time.now
    if t - @prev > 3
      @prev = t
      return "Hi"
    else
      nil
    end
  end
end
```

## はてなブックマークボットに記事の自動読み上げ機能を追加
[はてなブックマークのTOP5をスマホがしゃべりはじめるPictRuby botを書いた - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/hatena-bookmark-bot)にtimerを使った「自動読み上げ機能」を追加してサンプルに収録しました。

RubyPicoを起動して [Sample] -> [hatena_bookmark_bot.rb] を実行してください。[総合]で記事一覧を表示したのち、[all]と入力すると・・

[f:id:tuto0621:20160510222554g:plain]

ご覧のように記事のdescriptionをしゃべりはじます。

## iTunes経由でPCと端末間でスクリプトのコピーができるように
0.5からiTunes file sharingに対応していたのですがリリースのときに連絡を忘れていました。

- [iTunes file sharing を使ってPCからPictRubyのコードを書く - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/2016/04/13/235837)

## 過去の記事
- [PictRuby 0.5 リリース - チャットボット、eval、irb - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/pictruby-0.5)

