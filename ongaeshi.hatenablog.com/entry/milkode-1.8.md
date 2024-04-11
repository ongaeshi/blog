---
Title: Milkode 1.8 をリリース - キーボードショートカット、Herokuへのデプロイに対応、ファイル一覧の表示
Category:
- milkode
- groonga
- ruby
Date: 2014-06-30T10:14:11+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.8
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815727003054
---

- キーボードショートカットに対応
- Herokuへのデプロイに対応
- 'f:*'でパッケージ内のファイル一覧の表示

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.8.0)

## キーボードショートカットに対応
GitHubのようなキーボードショートカットに対応しました。[ヘルプ](http://try-milkode.herokuapp.com/help#keyboard_shortcut)。

- s .. 検索バーに移動
- S (テキスト選択) .. 選択したテキストを使って新しいタブで検索

<b>テキスト検索して'S'</b>がお気に入りです。
コピー→検索ボックスに移動→ペースト→検索ボタンをクリック
の流れが<b>'S'</b>一発で出来ます。関数を深く追う時などに便利です。

## Herokuへのデプロイに対応
HerokuにWebアプリがデプロイ出来るようになりました。

[ongaeshi/milkode-on-heroku](https://github.com/ongaeshi/milkode-on-heroku)

1. あらかじめ [Heroku Toolbelt](https://toolbelt.heroku.com/) を使えるようにしておきます
1. ソースコードをcloneして`PACKAGES`に読みたいソースコードを記述します
1. Webページの名前を変更したい時は`milkweb.yaml`を編集します
1. `git push heroku master` でデプロイされます。
1. ソースコードを追加したい時や設定を変更したい時は書き換えた後に再度herokuにpushして下さい

詳しくは[以前に書いた記事](http://ongaeshi.hatenablog.com/entry/milkode-on-heroku)をどうぞ。

## 'f:*'でファイル一覧を表示
'f:*' がパッケージ単位で表示出来るようになりました。
例えば['f:*' in milkode-on-heroku](http://try-milkode.herokuapp.com/home/milkode-on-heroku?query=f%3A*&shead=package)でmilkode-on-herokuのディレクトリをまたいだ全ファイルが一覧出来るようになります。

ソースコード読む時に、最初にファイルを一覧しながら簡単な用途を記述していくのに便利です。

## リリースノート
* milk web
  * Keyboard shortcuts
    * 's' .. Focus search bar
    * 'S' (Select text)  .. Search using a selected text with new tab
  * Add help of keyboard shortcut
  * Hide the favorite list if it is empty
  * Support for filtering by package name in 'f:*' search
  * Support :home_font_size attribute in milkweb.yaml

## 関連記事

- [Milkode 1.7 をリリース - gomilk](http://ongaeshi.hatenablog.com/entry/milkode-1.7)
- [Milkode1.4リリース - ドリルダウンによるマウスクリックの絞り込み機能を追加！](http://ongaeshi.hatenablog.com/entry/milkode-1.4)
- [Milkode1.3リリース - GitHubボタンを追加、キーワードのマッチ箇所をハイライト](http://ongaeshi.hatenablog.com/entry/milkode-1.3)
- [Milkode 1.2 リリース : ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート](http://ongaeshi.hatenablog.com/entry/milkode-1.2)

