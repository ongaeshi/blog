---
Title: RubyPico 0.9 リリース - gets、リンク付き文字列、クリックイベント、チュートリアル
Category:
- rubypico
Date: 2016-10-02T22:31:46+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.9
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687187456637
---

RubyPico 0.9 をリリースしました。getsによる入力、リンク付き文字列、クリックイベントのハンドリング、Ruby初心者のためのチュートリアルと盛りだくさんです。

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## gets
コンソールを表示しながら入力できるようになったのでより自然なirbが実装できました。

[f:id:tuto0621:20160924004944p:plain]

詳しくは[getsでirbを再実装](http://ongaeshi.hatenablog.com/entry/rubypico-diary-4)をどうぞ。

## リンク付き文字列
HTMLのように、見た目は普通の文字だけどクリックすると特定のURLにジャンプできる文字列を生成できるようになりました。

[f:id:tuto0621:20160924143357p:plain]

詳しくは[GitHubの気になるユーザーのレポジトリをiOSで一覧表示する](http://ongaeshi.hatenablog.com/entry/show-github-users-repos)をどうぞ。

## クリックイベント
さらにさらに、リンク付き文字列がクリックされたときに指定したコールバック関数が呼べるようになりました。つまりダミーのURLを埋め込んで実際にはRubyプログラムを呼び出すためのボタンを生成する・・ができるようになります！

サンプルタブに`click_link.rb`を用意したので参考にしてください。

```ruby
# # click_link
#
# ## Description
# Make clickable links

def a(str)
  AttrString.new(str, link: str)
end

def reload
  puts "Click bellow links\n\n"
  puts a("foo") + ", " + a("bar") + ", " + a("baz") + ", " + a("clear")
  puts "----"
end

TextView.click_link do |url|
  case url
  when "clear"
    TextView.clear
    reload
  else
    puts url
  end
end

TextView.click_link do |url|
  puts "Hi!, #{url}" if url == "bar"
end

reload
```

## チュートリアル
https://www.ruby-lang.org/ にある[「20分ではじめるRuby」](https://www.ruby-lang.org/ja/documentation/quickstart/)をRubyPicoでやるためのドキュメントを書きました。

[20分ではじめるRubyPico](http://rubypico.ongaeshi.me/ja/doc/quickstart/)

iOSが動く端末があれば0からRubyを学習することができます。

Rubyには[Try Ruby](http://www.tryruby.nl/)のようなブラウザだけですぐに試せる素晴らしいチュートリアルがたくさんあります。それらと比べてRubyPicoのよいところは自分のローカルマシンだけで動かせることです。もし無限ループのようなコードを間違えて書いてしまってもアプリを再起動するだけですみます。サーバートラブルで動かないといったこともありません。そしてなんといっても素晴らしいのはチュートリアルが終わったあとは自由に自分の好きなプログラムを書けるということです。早速RubyPicoを[ダウンロード](http://rubypico.ongaeshi.me/ja/download.html)してチュートリアルをお試しください。

誤字脱字、分かりにくい、感想などありましたら教えてもらえると助かります。
