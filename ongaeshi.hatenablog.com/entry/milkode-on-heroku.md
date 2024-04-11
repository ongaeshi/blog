---
Title: 30秒でHerokuにソースコード検索をデプロイできる Milkode on Heroku を作りました。
Category:
- milkode
- groonga
- ruby
- heroku
Date: 2014-06-16T13:47:12+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-on-heroku
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815726148282
---

[HerokuでRroongaが](http://www.clear-code.com/blog/2014/5/28.html)使えるようになったので、これはMilkodeもHerokuにデプロイ出来るのではないかと思いチャレンジしてみました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140615/20140615140636.png" alt="f:id:tuto0621:20140615140636p:plain" title="f:id:tuto0621:20140615140636p:plain" class="hatena-fotolife" itemprop="image"></span></p>


## インストール
[ongaeshi/milkode-on-heroku](https://github.com/ongaeshi/milkode-on-heroku)

- [使い方](https://github.com/ongaeshi/milkode-on-heroku#milkode-on-heroku)
- [デモアプリ](http://try-milkode.herokuapp.com/)

1. あらかじめ [Heroku Toolbelt](https://toolbelt.heroku.com/) を使えるようにしておきます
1. ソースコードをcloneして`PACKAGES`に読みたいソースコードを記述します
1. Webページの名前を変更したい時は`milkweb.yaml`を編集します
1. `git push heroku master` でデプロイされます。
1. ソースコードを追加したい時や設定を変更したい時は書き換えた後に再度herokuにpushして下さい

## 用法
- 自分の気になるソースコード置き場を作っていつでも読めるように (例: http://my-milkode.herokuapp.com/)
- グループワークなどでよく参考にするソースコードをまとめて置いておく
- 自分の環境にMilkodeをインストールせずに試してみたい

気になるソースコードを意味のある固まり(例えばある言語とプラグインや、アプリとその内部で使われているライブラリなど)でまとめておき、検索出来るようにしておくとなかなか便利です。




