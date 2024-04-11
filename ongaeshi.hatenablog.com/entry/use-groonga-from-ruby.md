---
Title: 国産の全文検索エンジンGroongaはRubyから簡単に使うことが出来る
Category:
- ruby
- groonga
- honyomi
Date: 2014-04-25T10:28:48+09:00
URL: https://ongaeshi.hatenablog.com/entry/use-groonga-from-ruby
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815722561611
---

[国産の全文検索エンジンGroonga vs 世界的流行のElasticsearch - CreateField Blog](http://blog.createfield.com/entry/2014/04/21/120023) 

がバズってるうちに(と、いいながらもう数日経ってしまった・・)関連情報を流してみようと思います。

## Rroonga
[Rroonga](http://ranguba.org/ja/)というgemを使うとRubyから簡単にGroongaの操作(データベースの作成、検索データの追加や削除、クエリーを使って検索など)を行うことが出来ます。

## GrnMini
自作の[GrnMini](https://github.com/ongaeshi/grn_mini)というgemを使うと検索インデックスの作成も含めてさらに簡単に実装することが出来ます。詳しくはリンク先のREADMEをどうぞ。

GrnMiniは内部でRroongaを使っているので以下で両方インストールすることが出来ます。

```
$ gem install grn_mini
```

## GrnMiniの実例
[Honyomi](http://ongaeshi.hatenablog.com/entry/honyomi-init)の内部で使っているデータベース＆検索エンジンはGrnMiniを使って書かれています。

本の集合である`Books`とページの集合である`Pages`が定義され、`Pages`から`Books`への参照が入っています。

```ruby
require 'honyomi'
require 'grn_mini'

module Honyomi
  class Database
    attr_reader :books
    attr_reader :pages

    def initialize
      @books = GrnMini::Array.new("Books")
      @pages = GrnMini::Hash.new("Pages")

      @books.setup_columns(path:     "",
                           title:    "",
                           author:   "",
                           page_num: 0,
                           timestamp: Time.new,
                           )
      @pages.setup_columns(book:    @books,
                           text:    "",
                           page_no: 0,
                           )
    end

    .
    .
end
```

[honyomi/lib/honyomi/database.rb](https://github.com/ongaeshi/honyomi/blob/master/lib/honyomi/database.rb) に全てのコードがありますが全部で<b>104行</b>しかありません。Groongaの表現力の高さを表すよい例なのではないかと思います。

104行の中に参照、書き換え、検索と一通り含まれているのでGroongaを勉強するサンプルとしても手頃なサイズなのではないでしょうか。





