---
Title: hatenablog gemでタグ付きの文章が出力できない問題を解決した
Category:
- diary
Date: 2016-08-03T00:20:33+09:00
URL: https://ongaeshi.hatenablog.com/entry/mr-to-hatenablog-writer
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687177249099
---

色々とコード書き換えながら試したいのでローカルにbundle installする。

   $ bundle install --path vendor/bundle

これで`vendor/bundle`以下にあるgemファイルを書き換えることでライブラリの挙動を変えたりpしながら調べることができる。

コードを読んでいくとxmlにエスケープが入っていないことが分かった。[rubyでシンプルなXML escape](http://techracho.bpsinc.jp/baba/2013_04_18/8101)。なので以下のようにエンコード関数を渡すことで動くようになった。

```ruby
def entry_xml(title = '', content = '', categories = [], draft = 'no', updated = '', author_name = @user_id)
  .
  .
  xml % [e(title), author_name, e(content), updated, categories_tag, draft]
end
```

足したe()関数はこんな感じ。

```ruby
def e(str)
  str.encode(xml: :text)
end
```

うまく行ったぜー、とプルリクエスト送ろうと思ったら[kymmt90/hatenablog](https://github.com/kymmt90/hatenablog)にあるコードと中身が違う、あれ？よくみるとbundle installで入っているのは`hatenablog-0.2.1`だけど最新は`hatenablog-0.2.2`であることに気がついた。これはもしかして・・・結局マージリクエストは以下のようになった。

[Update hatenablog to 0.2.2 by ongaeshi · Pull Request #2 · kymmt90/hatenablog-writer](https://github.com/kymmt90/hatenablog-writer/pull/2)

これで以下のようなタグ付きのテキストも問題なく投稿できるようになった。

```html
<html>
</html>
```
