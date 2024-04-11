---
Title: Ruby on Rails チュートリアルをやってみる 〜2章 デモアプリケーション〜
Category:
- ruby
- rails
Date: 2014-05-02T12:14:44+09:00
URL: https://ongaeshi.hatenablog.com/entry/railstutorial-02
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815723095641
---

<span style="font-size: 150%">[目次](http://ongaeshi.hatenablog.com/entry/railstutorial)
</span>

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140502/20140502121345.png" alt="f:id:tuto0621:20140502121345p:plain" title="f:id:tuto0621:20140502121345p:plain" class="hatena-fotolife" itemprop="image"></span></p>


2章では <b>scaffold</b> を使ったデモアプリケーションを作る。scaffold を使うのはこの章が最後で、以後は <b>rails new</b> の素の状態から一つずつビルドアップしていく。ここまでが旅立ち前の準備という所。

とりあえず始める前に Rails 用の rvm 設定に切り替え。

```
$ rvm 2.0.0@railstutorial_rails_4_0
```

教科書は[こちら](http://tatsu-zine.com/books/railstutorial)。

## 2.1 アプリの計画

まずアプリの作成。

```
$ rails new demo_app
```

で、 Gemfile を編集して bundle install する。

```
$ Gemfile の編集
$ bundle install --without production
$ bundle update
$ bundle install
```

`--without production` は <b>remembered option</b> と呼ばれていて、一回実行したら bundler が覚えてくれて以後オプションを設定する必要がなくなるらしい、便利。

このまま [GitHubにpush](https://github.com/ongaeshi/demo_app) して Heroku にデプロイする。このスピード感はすごいなぁ。

### 2.1.1 ユーザーのモデル設計  よくあるマイクロブログのモデルケース。

- users
  - id
  - name
  - email

- microposts
  - id
  - content
  - user_id

## 2.2 Uses リソース

ユーザーを HTTPプロトコル経由で 作成/読み出し/更新/削除 (CRUD) 出来る。 scaffold 使うとそれが1コマンドで出来る。

※ 最初にRailsを爆発的に普及させた一要素。この手の操作もう3〜4回はやってるなぁ・・。

```
$ rails generate scaffold User name:string email:string
$ bundle exec rake db:migrate
```

[REST - Wikipedia](http://ja.wikipedia.org/wiki/REST)

### 2.2.3 Users リソースの欠点
scaffold で生成された処理の欠点。特に一番下が大きい。(だから3〜4回も繰り返すことに・・)

- データの検証がない (validation)
- ユーザー認証が行われていない
- テストが行われていない
- レイアウトが整えられていない
- 理解が困難

## 2.3 Microposts リソース
### 2.3.2 マイクロポストをマイクロにする
### 2.3.3 ユーザーとマイクロポストを has_many で関連づける

**rails console** 便利だー。昔やった時はこの手の手動でデータモデルを使うのに MySQL 等の知識が必要ですごく難しかった印象。

```
$ rails console
>> first_user = User.first
>> first_user.microposts
```

### 2.3.5 デモアプリケーションのデプロイ

```
$ heroku create
$ git push heroku master
$ heroku run rake db:migrate
```

## 2章 感想
- remembered option なんて概念があるのか
- rails console 便利 (昔は無かった記憶)
- ローカルでは SQLite3 なのに heroku では PostgreSQL になっててかつコードの変更がほとんどないのは Rails (ActiveRecord?) の強みだなぁと思った

次章からはいよいよ scaffold から離れて 0 から Rails アプリを構築していく。

