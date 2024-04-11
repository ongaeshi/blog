---
Title: Rubyで簡単に全文検索エンジンが作れるGrnMiniを作った
Category:
- ruby
- groonga
- rroonga
- web
- programming
Date: 2014-01-06T15:34:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/create-grn-mini
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815715902994
---

<span style="font-size: 200%">[続き](http://ongaeshi.hatenablog.com/entry/full-text-search-150)を書きました。</span>

[RubyでただのArrayだと思って・・](http://ongaeshi.hatenablog.com/entry/ruby-grn-array)の続きです。正月中に整備してgem化しました。

## GrnMini

[ongaeshi/grn_mini](https://github.com/ongaeshi/grn_mini)

- Groonga(Rroonga)を簡単に使えるようにラップしたものです。
- カラム指定不要でデータを追加することが出来ます。
- 永続化、高度な検索クエリ、ソート、グループ化(ドリルダウン)、スニペット、ページネーションなどを簡単に使うことが出来ます。
- 検索エンジンがすぐに作れます。

## インストール

    $ gem install grn_mini

## 基本的な使い方

実体はRroongaの薄いラッパーですが難しいことを考えずに使えるよう工夫しています。

```ruby
require 'grn_mini'
array = GrnMini::Array.new("test.db")
```

初めてデータを追加する時にカラム種類を類推して作成します。追加するデータが文字列の時は合わせて全文検索用の転置インデックスを貼ります。

```ruby
array.add(text: "aaa", number: 1)
```

以後は同じカラム種類のデータを追加出来るようになります。データの追加は`<<`でも可能です。

```ruby
array << {text: "bbb", number: 2}
array << {text: "ccc", number: 3}
array.size  #=> 3
```

すでに作成済みのデータベースを開くことも出来ます。

```ruby
require 'grn_mini'
array = GrnMini::Array.new("test.db")
array.size   #=> 3
```

`GrnMini::Array#tmpdb`を使うとテンポラリデータベースを作成出来ます。(テストに便利です)

```ruby
GrnMini::Array.tmpdb do |array|
  array << {text: "aaa", number: 1}
  array << {text: "bbb", number: 2}
  array << {text: "ccc", number: 3}
end
# 一時的なデータベースが削除される
```

## データ種類

文字列、整数、小数、時間が今の所対応済みです。

```ruby
GrnMini::Array.tmpdb do |array|
  array << {filename: "a.txt", int: 1, float: 1.5, time: Time.at(1999)}
  array << {filename: "b.doc", int: 2, float: 2.5, time: Time.at(2000)}

  # ShortText
  array[1].filename #=> "a.txt"
  array[2].filename #=> "b.doc"

  # Int32
  array[1].int      #=> 1
  array[2].int      #=> 2

  # Float
  array[1].float    #=> 1.5
  array[2].float    #=> 2.5

  # Time
  array[1].time    #=> 1999-01-01
  array[2].time    #=> 2000-01-01
end
```

[8.4. データ型 — Groonga documentation](http://groonga.org/ja/docs/reference/types.html) も参考にして下さい。

## レコード操作
### 追加

```ruby
require 'grn_mini'

array = GrnMini::Array.new("test2.db")
array << {name:"Tanaka",  age: 11, height: 162.5}
array << {name:"Suzuki",  age: 31, height: 170.0}
```

### 読み込み

```ruby
record = array[1] # Read from id (> 0)
record.id         #=> 1
```

カラム名と同じ名前の関数でアクセス出来ます。

```ruby
record.name     #=> "Tanaka
record.age      #=> 11
record.height   #=> 162.5
```

`Groonga::Record#attributes` がデバッグに便利です。生のRroongaでも使えます。

```ruby
record.attributes #=> {"_id"=>1, "age"=>11, "height"=>162.5, "name"=>"Tanaka"}
```

### 更新

```ruby
array[2].name = "Hayashi"
array[2].attributes #=> {"_id"=>2, "age"=>31, "height"=>170.0, "name"=>"Hayashi"}
```

### 削除

IDを渡して削除します。

```ruby
array.delete(1)

# It returns 'nil' value when you access a deleted record
array[1].attributes     #=> {"_id"=>1, "age"=>0, "height"=>0.0, "name"=>nil}

# Can't see deleted records if access from Enumerable
array.first.id          #=> 2
array.first.attributes  #=> {"_id"=>2, "age"=>31, "height"=>170.0, "name"=>"Hayashi"}
```

ブロックを渡して削除することも出来ます。

```ruby
GrnMini::Array.tmpdb do |array|
  array << {name:"Tanaka",  age: 11, height: 162.5}
  array << {name:"Suzuki",  age: 31, height: 170.0}
  array << {name:"Hayashi", age: 20, height: 165.0}

  array.delete do |record|
    record.age <= 20
  end

  array.size             #=> 1
  array.first.attributes #=> {"_id"=>2, "age"=>31, "height"=>170.0, "name"=>"Suzuki"}
end
```

## 検索

生成したテーブルの検索には`GrnMini::Array#select`を使います。特に指定しない場合は`:text`カラムが`:default_column`指定されます。

```ruby
GrnMini::Array.tmpdb do |array|
  array << {text:"aaa", number:1}
  array << {text:"bbb", number:20}
  array << {text:"bbb ccc", number:2}
  array << {text:"bbb", number:15}
  array << {text:"ccc", number:3}

  results = array.select("aaa")
  results.map {|record| record.attributes} #=> [{"_id"=>1, "_key"=>{"_id"=>1, "number"=>1, "text"=>"aaa"}, "_score"=>1}]

  # AND検索
  results = array.select("bbb ccc")
  results.map {|record| record.attributes} #=> [{"_id"=>2, "_key"=>{"_id"=>3, "number"=>2, "text"=>"bbb ccc"}, "_score"=>2}]

  # カラム指定
  results = array.select("bbb number:<10")
  results.map {|record| record.attributes} #=> [{"_id"=>2, "_key"=>{"_id"=>3, "number"=>2, "text"=>"bbb ccc"}, "_score"=>2}]

  # AND, OR, グルーピング
  results = array.select("bbb (number:<= 10 OR number:>=20)")
  results.map {|record| record.attributes} #=> [{"_id"=>2, "_key"=>{"_id"=>3, "number"=>2, "text"=>"bbb ccc"}, "_score"=>2}, {"_id"=>4, "_key"=>{"_id"=>2, "number"=>20, "text"=>"bbb"}, "_score"=>2}]

  # NOT
  results = array.select("bbb - ccc")
  results.map {|record| record.attributes}  #=> [{"_id"=>1, "_key"=>{"_id"=>2, "number"=>20, "text"=>"bbb"}, "_score"=>1}, {"_id"=>3, "_key"=>{"_id"=>4, "number"=>15, "text"=>"bbb"}, "_score"=>1}]
end 
```

以下は `:default_column` を `:filename` に変更する例です。

```ruby
GrnMini::Array.tmpdb do |array|
  array << {text: "txt", filename:"a.txt"}
  array << {text: "txt", filename:"a.doc"}
  array << {text: "txt", filename:"a.rb"}

  # カラム指定する場合
  results = array.select("filename:@txt")
  results.first.attributes  #=> {"_id"=>1, "_key"=>{"_id"=>1, "filename"=>"a.txt", "text"=>"txt"}, "_score"=>1}

  # デフォルトカラムを変更する場合
  results = array.select("txt", default_column: "filename")
  results.first.attributes  #=> {"_id"=>1, "_key"=>{"_id"=>1, "filename"=>"a.txt", "text"=>"txt"}, "_score"=>1}
end
```

さらなる情報は [8.10.1. クエリー構文](http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html), [Groonga::Table#select](http://ranguba.org/rroonga/ja/Groonga/Table.html#select-instance_method) を参考にして下さい。

Groongaのクエリー構文はかなり充実しているので、リファレンスマニュアルを読めばやりたいことは一通り出来るのではないかと思います。

## その他の機能
[README.md](https://github.com/ongaeshi/grn_mini/blob/master/README.md)をどうぞ。

ソート、グルーピング、スニペット、ページネーションなど検索エンジンを作るのに必要な機能は一通り揃っています。

## 作った経緯

セブンイレブンのコーヒーが去年一番のヒット商品だそうですね。家にコーヒーのドリップセットがあるのでそんなに利用することは無いのですが、話を聞くとそこまでするのは面倒だけど100円で美味しいコーヒーを飲めるというのがヒットの秘訣らしいです。確かにドリップしたり豆を挽くのは結構面倒です。(我が家にも面倒な時用のインスタントコーヒーがあります、本末転倒ですが・・)

ドリップや豆を挽くなどはコーヒーを美味しく飲む方法としてはむしろ初心者向けです。さらに美味しく飲みたければ自家焙煎やエスプレッソマシンなどいくらでもあるのですが手間とお金がどんどん増えます。究極に美味しいコーヒーを飲む方法も大切ですが、誰でも簡単に美味しいコーヒーが飲めるための仕組みも大切だと思うのです。

長い前置きがあってここからが本題なのですが、Rroongaという全文検索エンジンがあって、日本語の検索に強くて、ストレージとしても使えて、日本語の情報も多くて愛用しています。これをもっと簡単に、たくさんの人に使ってもらいたくてGrnMiniを作りました。

特にカラムストア系のデータベースでは、<b>カラムの型種類指定と全文検索用の転置インデックスを格納するためのテーブルの作成</b>が初心者にとっては複雑で難しいです。逆に言えばこれが自動化されれば最近流行っているKVSとそんなに変わらず使えるのではないかと思っています。

GrnMiniを使うと配列と同じ感覚で高速な全文検索の恩恵を受けることが出来ます。薄いラッパーなので生のRroongaの機能を直接使うのも簡単です。是非使ってみて感想をお聞かせ下さい。



