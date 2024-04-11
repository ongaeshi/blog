---
Title: PictRubyでスマホプログラミング - ブログテンプレートエンジンを作る
Category:
- rubypico
- ruby
- ios
Date: 2016-02-09T20:00:00+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-blog-template
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347156613703
---

PictRubyプログラムの紹介記事を自動生成します。(この記事自体もブログテンプレートを使って作成されました。)

[f:id:tuto0621:20160209143606p:image:w300]

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## ソースコード

以下をコピペしてPictRubyに貼り付けてください。

```ruby
# blog_template.rb

def main
  title = Popup.input "Title?"
  summary = Popup.input "Summary?"
  usage = Popup.input "Usage?"
  
  template title, summary, usage
end

def template(title, summary, usage)
  <<EOF
# PictRubyでスマホプログラミング - #{title}


#{summary}

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## ソースコード

以下をコピペしてPictRubyに貼り付けてください。

[get_sample.rb](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)を使って``でも取得するとこができます。

# ↓`の前のスペースは除去してください
 ```ruby
#{Clipboard.get}
 ```

## 使い方

#{usage}


スクリプトは[ランチャーアプリ経由](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)で通知センターに登録しておくとさらに便利です。


EOF
end
```

## 使い方

あらかじめ紹介したいソースコードをコピーしておきます。実行してタイトル、概要、使い方を入力します。複数行の入力には`Clipboard.get`を使うのがポイントです。


スクリプトは[ランチャーアプリ経由](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)で通知センターに登録しておくとさらに便利です。


