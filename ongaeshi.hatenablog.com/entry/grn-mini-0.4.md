---
Title: ' GrnMiniでテーブルを連携させてTwitterぽいものが作れるように'
Category:
- ruby
- rroonga
- groonga
- grn_mini
Date: 2014-02-03T16:09:49+09:00
URL: https://ongaeshi.hatenablog.com/entry/grn-mini-0.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815717637438
---

<span style="font-size: 150%">[続き](http://ongaeshi.hatenablog.com/entry/grn-mini-restaurant) を書きました。</span>

今まではデータベース1つにつき1つのテーブルしか持つことが出来ませんでした。0.4では複数のテーブルを作ったりテーブル間を連携させて複雑なデータ構造を作れるようになりました。 いよいよカラムデータベースの本領発揮です。

## GrnMini 0.4.0 の特徴

https://github.com/ongaeshi/grn_mini

- 複数テーブルの作成
- テーブル間の参照を可能に
- Hash型をサポート

## インストール

    $ gem install grn_mini


## 複数テーブルの作成

GrnMini::Array.new(name) でテーブル名を指定します。デフォルトは"Array"です。

データベースの作成には GrnMini.create_or_open(name) を使うようになりました。

```ruby
require 'grn_mini'

# データベース "test.db" を作成
GrnMini::create_or_open("test.db")

# テーブル名 "Array" で作成
array = GrnMini::Array.new

# テーブル名 "Users" で作成
users = GrnMini::Array.new("Users")
```

## テンポラリデータベースの作成

GrnMini::Array.tmpdb から GrnMini.tmpdb になりました。

```ruby
GrnMini::tmpdb do
  array = GrnMini::Array.new
  users = GrnMini::Array.new("Users")

  array << {text: "aaa", number: 1}
  array << {text: "bbb", number: 2}
  array << {text: "ccc", number: 3}
end
```

## Hash型のサポート

配列に続いてハッシュ型(GrnMini::Hash)も作成出来るようになりました。キー経由での高速な読み書きが可能です。

```
require 'grn_mini'
GrnMini::create_or_open("test.db")
hash = GrnMini::Hash.new

# Add
hash["a"] = {text:"aaa", number:1}
hash["b"] = {text:"bbb", number:2}
hash["c"] = {text:"ccc", number:3}

# Read
hash["b"].text       #=> "bbb"

# Write
hash["b"].text = "BBB"
```

## setup_columns

ArrayやHashで明示的にカラム種類を指定することが出来るようになりました。カラム名に格納したいデータ種類を指定することで表現します。

```ruby
GrnMini::tmpdb do
  array = GrnMini::Array.new

  # 格納したいデータ種類を指定
  array.setup_columns(filename: "",
                      int:      0,
                      float:    0.0,
                      time:     Time.new,
                      )

  array << {
    filename: "a.txt", 
    int: 1,
    float: 1.5, 
    time: Time.at(1999)
  }
end
```

上記のコードは以下と同じです。

```ruby
users = GrnMini::Array.new("users")

GrnMini::tmpdb do
  array = GrnMini::Array.new

  # 最初にデータを作成した時にカラム種類が設定される
  array << {
    filename: "a.txt", 
    int: 1,
    float: 1.5, 
    time: Time.at(1999)
  }
end
```

なんで明示的に指定する必要があるの？という話ですが以下の時に便利です。

- カラム要素に自分自身や他のテーブルを持たせたいとき

続きます。

## テーブル間の参照を可能に
以下のようにsetup_columnsに<b>作成したテーブル型を渡す</b>ことでテーブル間の参照を表現します。配列で囲むとベクターカラムになります。

```ruby
GrnMini::tmpdb do
  articles = GrnMini::Hash.new("Articles")   # 記事が格納されるテーブル
  users = GrnMini::Hash.new("Users")         # ユーザーが格納されるテーブル

  articles.setup_columns(author: users,      # 単一のUsersテーブルを保持
                         text: ""
                         )

  users.setup_columns(name: "",
                      favorites: [articles]  # 配列で囲むとベクターカラムになる、複数のArticlesテーブル要素を保持
                      )
end
```

## マイクロブログの例

簡単なマイクロブログの例です。この短さでテーブル定義、データの追加、検索まで出来ます。すごいでしょ？

```ruby
GrnMini::tmpdb do
  users = GrnMini::Hash.new("Users")
  articles = GrnMini::Hash.new("Articles")

  users.setup_columns(name: "",
                      favorites: [articles]
                      )

  articles.setup_columns(author: users,
                         text: ""
                         )

  users["aaa"] = {name: "Mr.A"}
  users["bbb"] = {name: "Mr.B"}
  users["ccc"] = {name: "Mr.C"}

  articles["aaa:1"] = {author: "aaa", text: "111"}
  articles["aaa:2"] = {author: "aaa", text: "222"}
  articles["aaa:3"] = {author: "aaa", text: "333"}
  articles["bbb:1"] = {author: "bbb", text: "111"}
  articles["bbb:2"] = {author: "bbb", text: "222"}
  articles["ccc:1"] = {author: "ccc", text: "111"}

  users["aaa"].favorites = ["aaa:1", "bbb:2"]
  users["bbb"].favorites = ["aaa:2"]
  users["ccc"].favorites = ["aaa:1", "bbb:1", "ccc:1"]

  # Search record.favorites
  users.select { |record| record.favorites == "aaa:1" }    #=> ["aaa", "ccc"]
  users.select { |record| record.favorites == "aaa:2" }  #=> ["bbb"]

  # Search record.favorites.text
  users.select { |record| record.favorites.text =~ "111" } #=> ["aaa", "ccc"]
end
```

さらに詳しい使い方は[README.md](https://github.com/ongaeshi/grn_mini)をご覧下さい。

## GrnMiniでやりたいこと

小さなアプリケーションでも全文検索のテクノロジーが使えると便利になることはたくさん存在すると思っているですが、現状では全文検索やデータベースといったテクノロジーは大勢のユーザーに使われるWebアプリ向き、という印象があって手軽に使えるとはいえない印象です。

GrnMiniは複雑な設定やオプションを意識せずにとにかく簡単に使えることを目標に作っています。(慣れて来て細かいカスタマイズが必要になったら直接Rroongaを叩くことも出来ます。)

良くある全文検索エンジンでは 1.エンジン本体のインストール 2.操作するためのgemをインストール で<b>2手かかる</b>パターンが多いのですが、GrnMiniではRroongaがインストール時に一緒にGroongaもインストールされるため、gemのインストールだけですぐに使えるようになります。

正規表現がPerlやRubyに組み込まれることで様々な用途に使われるようになったように、GrnMiniを使うことで誰もが全文検索を使えるようになればいいなぁと思っています。高速に検索出来るストレージとしてもお使い下さい。
