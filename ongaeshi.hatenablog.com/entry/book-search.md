---
Title: 「本」と「読書」にまつわる便利なサイトをスマホから横断検索する
Category:
- rubypico
- book
- ruby
- ios
- programming
Date: 2016-03-03T23:23:20+09:00
URL: https://ongaeshi.hatenablog.com/entry/book-search
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792365663867
---

[「本」と「読書」にまつわる便利＆参考になるサイトをまとめてみた - ぐるりみち。](http://yamayoshi.hatenablog.com/entry/2016/03/02/203125)がとても面白かったです。記事自体も素晴らしいし、はてぶのコメントも、ここには載ってないけどこのサイトもおすすめ、ほとんど知ってたけどこれは知らなかった、などよいコメントが並んではてなブックマークの楽しさを再認識しました。

どうせなら特定のキーワードでまとめて横断検索できたらさらに便利になりそうだと思いPictRubyのスクリプトを書きました。よかったら使ってやってください。

[f:id:tuto0621:20160303233158g:plain]

## インストール
[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## ソースコード
以下をコピペしてPictRubyに貼り付けてください。

[get_sample.rb](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)を使って`ja/book_search`でも取得するとこができます。

```ruby
# # ja/book_search
#
# ## 概要
# 本にまつわる便利サイトからまとめて検索できます。
# テイクストックは検索後に地域を指定してください。
# 想は右のCLEARボタンを押したあとにクリップボードから貼り付け。
# 本の書き出しはジャンプするだけです。

def main
  word = Popup.input("検索ワード？")
  Clipboard.set(word)
  word = URI.encode_www_form_component(word)
  
  puts <<EOF
テイクストック
https://takestock.jp/searches?q=#{word}

カーリル
http://calil.jp/search?q=#{word}

想
http://imagine.bookmap.info

本の書き出し
http://kakidashi.com

ダ・ヴィンチニュース
http://ddnavi.com/?s=#{word}

HONZ
http://honz.jp/search/?ie=UTF-8&page=1&fulltext=#{word}

bookvinegar
http://www.bookvinegar.jp/search/word/?q=#{word}

読書メーター
http://i.bookmeter.com/s?q=#{word}

本が好き！
http://www.honzuki.jp/smp/book/new_search/?search_word=#{word}
EOF
end
```

## 使い方
入力された単語で本と読書にまつわる便利なサイトを横断検索します。

- 「テイクストック」は検索後に地域を指定してください。
- 「想」は右のCLEARボタンを押したあとにクリップボードから貼り付け。(検索ワードは自動でクリップボードにコピーされます)
- 「本の書き出し」はジャンプするだけです。(でもたまに見ると楽しい)

スクリプトは[ランチャーアプリ経由](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)で通知センターに登録しておくとさらに便利です。

よく使うサイトを追加したり、自分の好きな順に並び替えたり、自分の使いやすい形にカスタマイズしていきましょう。

