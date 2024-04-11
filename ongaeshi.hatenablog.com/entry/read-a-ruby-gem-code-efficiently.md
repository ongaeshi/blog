---
Title: Rubyのgemのソースコードを効率的に読む方法
Category:
- ruby
- gem
- bundler
- milkode
- tag
- diary
Date: 2015-01-30T01:00:15+09:00
URL: https://ongaeshi.hatenablog.com/entry/read-a-ruby-gem-code-efficiently
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450081795667
---

いきなり読み始めてもよいのですが、事前に軽く準備しておくと読みやすくなります。

- 読みたいソースコードをダウンロード
- bundle install --path vendor/bundle
- 検索用のインデックスを貼る
- 読む

[bgm.rb](http://hitode909.hatenablog.com/entry/2015/01/24/173901)を例に説明します。

## 読みたいソースコードをダウンロード
[hitode909/bgm](https://github.com/hitode909/bgm)

```
$ git clone git@github.com:hitode909/bgm.git
$ cd bgm
```

## bundle install --path vendor/bundle


```
$ bundle install --path vendor/bundle
.
.
Installing json 1.8.2
Installing multi_xml 0.5.5
Installing httparty 0.13.3
Installing itunes-search-api 0.1.0
Using bundler 1.7.12
Your bundle is complete!
It was installed into ./vendor/bundle
Post-install message from httparty:
When you HTTParty, you must party hard!
```

これで`vendor/bundle`以下にgemのソースコードがインストールされます。

書き換えやすくするためにgitにもコミットしておきます。人のコードなのでアクセス権も無いはずなので間違えてpushすることもないはず。(身内のコードだとあるかもしれないので注意。その場合はブランチ切りましょう)

```
$ git add .
$ git commit -m "Add vendor/bundle"
```

## 検索用のインデックスを貼る
私の場合はMilkodeの検索インデックスとctags(Emacs用)を貼ります

```
$ milk add .
package    : bgm
github     : hitode909/bgm
result     : 1 packages, 237 records, 237 add. (7.45sec)
*milkode*  : 24 packages, 9032 records in /Users/ongaeshi/.milkode/db/milkode.db.
```

タグファイルは`milk update`と一緒に更新されるように設定します。

```
$ milk config update_with_ctags_e true
$ milk update
```

## 読む
これで準備完了です。後はそれぞれ好きな方法で。

私は始めにMilkodeのWebアプリを立ち上げます。ざっとブラウザでコードの概要を掴んでからエディタで細部を読んでいきます。関数の間はタグジャンプで移動して、この関数はどこから呼ばれているのか？などはMilkodeで検索して見つけます。

```
$ milk web
```

関連データは全てディレクトリ以下にあるのでソースコードに直接日本語のメモも書きます。書き込んだ場所はgit diffで取ることも出来ますしキリのよい所でコミットも出来るので便利です。

## まとめ
手順をまとめて書くと以下のようになります。

```
git clone git@github.com:hitode909/bgm.git
cd bgm
bundle install --path vendor/bundle
git add .
git commit -m "Add vendor/bundle"
milk add .
milk config update_with_ctags_e true
milk update
```

割とスクリプト化出来そうな気がしてきました。次回はbgm.rbを読んでいきます。
