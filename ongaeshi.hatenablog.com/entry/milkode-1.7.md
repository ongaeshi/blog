---
Title: Milkode 1.7 をリリース - gomilk
Category:
- ruby
- go
- groonga
- milkode
Date: 2014-06-03T10:28:46+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.7
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815725424707
---

- Go言語で作ったgmilkの高速版、gomilkに対応
- gmilk --expand-path に対応
- README, ヘルプの国際化強化

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.7.0)


## Go言語で作ったgmilkの高速版、gomilkに対応
[ongaeshi/gomilk](https://github.com/ongaeshi/gomilk)

Go言語で書かれた高速なgmilkです。

Rubyで書かれたWebアプリと通信して(Groongaを使って)検索候補となるファイルを見つけ出し、Go言語でファイル内検索するのでめちゃ早いです。

試しにRubyのソースコード全体(4722 files)から検索すると0.2秒位で結果が返ってきます。Linux(45752 files)でも0.5秒位でした。- [詳しく](https://github.com/ongaeshi/gomilk#performance-test)

ファイル内検索部分は [monochromegane/the_platinum_searcher](https://github.com/monochromegane/the_platinum_searcher) を大分参考にさせて頂きました。私の場合はよく検索するコード群はMilkodeに登録して`gomilk`で検索し、それ以外は`pt`を使って検索するように使い分けています。

### 簡単な使い方
1. gomilkのバイナリをダウンロードして(またはソースからgo build)、PATHの通った場所に置きます。
2. `mil web -g`でWebアプリを起動します
3. 後はgmilkと同じように使って下さい。

```
# 現在パッケージで検索
$ gomilk search_keyword

# 全体パッケージで検索
$ gomilk -a search_keyword
```

詳しくは[ongaeshi/gomilk](https://github.com/ongaeshi/gomilk)をどうぞ。

[emacs-milkode](https://github.com/ongaeshi/emacs-milkode#use-gomilk)で使う時は`--nogroup --nocolor --smart-case`オプションを追加するといいです。

## gmilk --expand-path に対応

[gmilkでの検索結果のファイルのパス · Issue #66 · ongaeshi/milkode](https://github.com/ongaeshi/milkode/issues/66)


## README, ヘルプの国際化強化

READMEに基本的な使い方を(英語で)追加しました。
英語版のヘルプも書きました。これでWebアプリで日本語しか無い所は無くなったはず・・。

## リリースノート
* milk web
  * Support gomilk option (milk web -g)
    * https://github.com/ongaeshi/gomilk
  * Support i18n in the help.haml

* gmilk
  * gmilkでの検索結果のファイルのパス (thanks kazuna)

* etc
  * Improve README
  * Add a badge of gem version
  * Change the setting of TravisCI [groonga-dev,02268]
  * Change to Bundler a method of generating a gem
    * Jeweler, Thanks for long support

## 関連記事

- [Milkode 1.6.1 をリリースしました](http://ongaeshi.hatenablog.com/entry/milkode-1.6.1)
- [Milkode1.4リリース - ドリルダウンによるマウスクリックの絞り込み機能を追加！](http://ongaeshi.hatenablog.com/entry/milkode-1.4)
- [Milkode1.3リリース - GitHubボタンを追加、キーワードのマッチ箇所をハイライト](http://ongaeshi.hatenablog.com/entry/milkode-1.3)
- [Milkode 1.2 リリース : ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート](http://ongaeshi.hatenablog.com/entry/milkode-1.2)

