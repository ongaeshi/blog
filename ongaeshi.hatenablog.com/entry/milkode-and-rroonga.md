---
Title: Milkodeで作ったデータベースをRroongaで開いて簡単に全文検索する
Category:
- groonga
- ruby
- milkode
- rroonga
Date: 2014-12-18T03:18:12+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-and-rroonga
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450077363363
---

この記事は[Groonga Advent Calendar 2014 - Qiita](http://qiita.com/advent-calendar/2014/groonga)の17日の記事です。

Groongaはオープンソースのカラムストア機能付き全文検索エンジンです。Rroongaや[GrnMini](https://github.com/ongaeshi/grn_mini)を使うとRuby経由で簡単にアクセスすることが出来ます。

検索用クエリを工夫してデータを色々な角度から眺めるのは楽しいのですが、全文検索をするスクリプトを書く時に面倒なのは全文検索用のデータベースを用意する部分です。

そこで、

- データベースの作成は他のアプリケーションを使って行い
- 検索部分だけを自力で書く

というアイデアを思いつきました。

## Milkodeのインストール
今回はソースコード検索エンジンのMilkodeを使います。ソースコード検索エンジンと詠っていますが本質的にはファイルデータベースなので好きなファイル群を登録してRubyスクリプトから検索する、といったことが出来ます。

[Milkode - 行指向のソースコード検索エンジン](http://milkode.ongaeshi.me/)

```
$ gem install milkode
```

試しにrailsのソースコードでも足しておきましょう。

```
$ milk init
$ milk add -p git https://github.com/rails/rails.git
```

## データベースを開く

データベースを開いて検索してみます。`/Users/ongaeshi/.milkode`は適宜置き換えて下さい。

```ruby
# -*- coding: utf-8 -*-
require 'groonga'

# データベースを開く
Groonga::Database.open("/Users/ongaeshi/.milkode/db/milkode.db")

# テーブルを開く
documents = Groonga["documents"]
p documents.count               # => 登録されているレコードの数: 2733

# レコードの内容を読む
path = "/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/relation/calculations.rb"
p documents[path].path          # => 絶対パス: "/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/relation/calculations.rb"
p documents[path].package       # => パッケージ名: "rails"
p documents[path].restpath      # => パッケージ名より先のファイルパス: "activerecord/lib/active_record/relation/calculations.rb"
p documents[path].content       # => ファイルの内容: "module ActiveRecord\n  module Calculations..."
p documents[path].timestamp     # => タイムスタンプ: 2014-12-18 02:25:11 +0900
p documents[path].suffix        # => 拡張子: "rb"
puts

# 'column:@''は部分一致、'column:'は完全一致
# http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html

# ファイル内容に以下のキーワードを全て含むレコードを探す
documents.select("content:@def content:@pluck").each do |record|
  p record.path
end
puts

# 内容に'def','pluck'を含み拡張子'md'のもの
documents.select("content:@def content:@pluck suffix:md").each do |record|
  p record.path
end
puts

# パッケージ名'rails'で拡張子'rdoc'
documents.select("package:rails suffix:rdoc").each do |record|
  p record.path
end
puts

# 内容に'pluck'を含みファイルパスに'cases'を含むもの
documents.select("content:@pluck restpath:@cases").each do |record|
  p record.path
end
puts
```

実行結果

```
2733
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/relation/calculations.rb"
"rails"
"activerecord/lib/active_record/relation/calculations.rb"
"module ActiveRecord\n  module Calculations\n   ..."
2014-12-18 02:25:11 +0900
"rb"

"/Users/ongaeshi/.milkode/packages/git/rails/actionview/lib/action_view/routing_url_for.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/associations/collection_association.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/null_relation.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/querying.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/relation/calculations.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/relation/finder_methods.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/schema_migration.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/lib/active_record/scoping/named.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_and_belongs_to_many_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_many_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_many_through_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/base_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/calculations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relation/merging_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relation_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/models/post.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/3_2_release_notes.md"
"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/action_mailer_basics.md"
"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/active_record_querying.md"

"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/3_2_release_notes.md"
"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/action_mailer_basics.md"
"/Users/ongaeshi/.milkode/packages/git/rails/guides/source/active_record_querying.md"

"/Users/ongaeshi/.milkode/packages/git/rails/actionmailer/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/actionpack/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/actionview/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/actionview/RUNNING_UNIT_TESTS.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/activemodel/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/RUNNING_UNIT_TESTS.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/activesupport/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/railties/lib/rails/generators/rails/app/templates/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/railties/lib/rails/generators/rails/plugin/templates/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/railties/RDOC_MAIN.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/railties/README.rdoc"
"/Users/ongaeshi/.milkode/packages/git/rails/RELEASING_RAILS.rdoc"

"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_and_belongs_to_many_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_many_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/associations/has_many_through_associations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/base_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/calculations_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relation/merging_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relation_test.rb"
"/Users/ongaeshi/.milkode/packages/git/rails/activerecord/test/cases/relations_test.rb"
```

## 応用: ファイルマッチャー

引数に全てマッチするファイルだけを出力するスクリプト。helmやanythingのインターフェースに使えそう。

```ruby
require 'groonga'

Groonga::Database.open("/Users/ongaeshi/.milkode/db/milkode.db")
documents = Groonga["documents"]

query = ARGV.map { |e| "restpath:@#{e}" }.join(" ")

documents.select(query).each do |record|
  puts record.path
end
```

```
$ ruby filematcher.rb slice hash
/Users/ongaeshi/.milkode/packages/git/rails/activesupport/lib/active_support/core_ext/hash/slice.rb
```

## まとめ
ファイル情報にRubyスクリプト経由で高速にアクセス出来るので割と応用範囲は広いのではないかと考えています。面白い使い方を見つけたら是非ブログやQiitaなどに書いてくれると(私が)喜びます。

[Honyomi](https://github.com/ongaeshi/honyomi)など他のGroongaデータベースを使っているアプリケーションでも同じことが出来るのではないかと思います。


