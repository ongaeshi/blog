---
Title: ' Groonga(Rroonga)で検索時に特定カラムに重みを付けたい'
Category:
- ruby
- groonga
Date: 2014-11-13T01:16:26+09:00
URL: https://ongaeshi.hatenablog.com/entry/groonga-set-weight-to-column
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450073363426
---

例えばあるテーブルに `title`(タイトル), `content`(本文), `comment`(コメント) カラムがあって

- タイトルには20倍
- 本文には10倍

の重みを付けたい場合は以下のようにする。

```ruby
grn.select do |record|
  record.match("aaa") do |target|
    (target.title * 20) | (target.content * 10)
  end
end
```

`"aaa"`の所は[検索クエリ](http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html)を指定する。

これでタイトルや本文に`"aaa"`が含まれる要素が検索結果の上位に表示されるようになる。

参考: [[groonga-dev,02944] Rroongaで検索時に特定カラムに重みを付けたい](http://sourceforge.jp/projects/groonga/lists/archive/dev/2014-November/002946.html)
