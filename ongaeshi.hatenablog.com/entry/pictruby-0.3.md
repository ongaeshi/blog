---
Title: iOSのRubyプログラミング環境、PictRuby 0.3 をリリースしました
Category:
- rubypico
- ruby
- ios
Date: 2016-01-24T21:40:32+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-0.3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347154401856
---

PictRuby 0.3 をリリースしました。

- Rubyの標準ライブラリをたくさん追加
- クラスブラウザ
- エントリポイントの関数名をmainに
- URL Schemeに対応
- 等幅フォント

[PictRuby - Ruby Programming Environment in iOS](http://pictruby.ongaeshi.me/)

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## Rubyの標準ライブラリをたくさん追加
mrubyを[ios-ruby-embedded](https://github.com/ongaeshi/ios-ruby-embedded)から作成するようにしました。mrbgemsが使えるようになりライブラリの組み込みが簡単になりました。例えば以下のような機能が使えるようになります。

- Array#-
- ”あ”.size==1
- rand

## クラスブラウザ
追加された標準ライブラリを駆使してクラス一覧やクラスで呼び出されるメソッド一覧を取得できるスクリプトをmrubyを使って作ってみました。メタプログラミングですね。

- [get_sample](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)をインストール
- get_sampleを起動して`09_class_help`と入力
- スクリプトをコピペしてインストール

起動してCancelを押せばクラス一覧、クラス名を入力してOKを押せばクラスメソッドの一覧が表示されます。クラス名はクラス一覧からコピペするのが便利です。

[f:id:tuto0621:20160124215017p:plain]
[f:id:tuto0621:20160124215020p:plain]
[f:id:tuto0621:20160124215025p:plain]

## エントリポイントの関数名をmainに
スクリプトのエントリポイントの関数名をconvertからmainに変更しました。当面はconvertでも動くようにしますが今後はmainが推奨です。

```ruby
# Please return the text or image in the "def main"

def main
  Popup.msg("Hi!")
  "Hello, PictRuby\nhttp://pictruby.ongaeshi.me"
end
```

## URL Schemeに対応
URL Scheme経由でスクリプトを実行できるようになりました。[Launcher](http://www.cromulentlabs.com/launcher.html)などでPictRubyのURLを指定することでスクリプトを直接起動できるようになります。例えば`hello.rb`という名前のスクリプトをURL Scheme経由で起動したい時は以下のように指定します。

```
pictruby://hello
```

.rbはあってもなくてもいいです。省略推奨です。

## 等幅フォント
エディタ画面と実行結果画面のフォントを等幅フォントに変えたので表組みなどがやりやすくなりました。

## おまけ
少しずつ使ってくれる人が増えてきて嬉しい限りです。

- [「PictRuby」iOS用のプログラミング言語Rubyがv0.3にアップデート | ひとりぶろぐ](http://hitoriblog.com/?p=34578)
  - 写真付きで詳しく紹介されています
- [Ruby初心者が無謀にもPictRubyで写真リサイズに挑戦してみた - W&R : Jazzと読書の日々](http://d.hatena.ne.jp/wineroses/20160124/p1)
  - テキストエディタのTextwellと組み合わせて画像の縮小に使っているようです
- [PictRubyGemsのスクリプトをPictRubyアプリから読む - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)
  - 大量のサンプルコードをアプリから読むことができます
  - Pull Request 絶賛募集中です

## 過去の記事
- [iOSでRubyプログラミングできるPictRuby 0.2 - アプリのランチャーになったりWebAPIを叩けるようになった](http://ongaeshi.hatenablog.com/entry/pictruby-0.2)
