---
Title: ' Milkode1.4リリース - ドリルダウンによるマウスクリックの絞り込み機能を追加！'
Category:
- milkode
- groonga
- ruby
Date: 2013-12-03T10:06:44+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815713680585
---

[f:id:tuto0621:20131202210419g:plain]

- ドリルダウンで拡張子、プロジェクト、ディレクトリの絞り込み
- 国際化
- milk add -b オプションでブランチ指定
- gmilkの出力モードにSJISを追加

最新のMilkodeを試してみたい時は[mruby Code Search](http://mruby-code-search.ongaeshi.me/)をどうぞ。

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.4.0)

## ドリルダウンで拡張子、プロジェクト、ディレクトリの絞り込み
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131202/20131202205750.png" alt="f:id:tuto0621:20131202205750p:plain" title="f:id:tuto0621:20131202205750p:plain" class="hatena-fotolife" itemprop="image"></span></p>


マウスクリックだけで絞り込みが出来るようになりました。

## 国際化
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131202/20131202205717.png" alt="f:id:tuto0621:20131202205717p:plain" title="f:id:tuto0621:20131202205717p:plain" class="hatena-fotolife" itemprop="image"></span></p>
[support I18N (not finished) by takahashim · Pull Request #2](https://github.com/ongaeshi/mruby-code-search/pull/2)

[@takahashim](https://github.com/takahashim)さんのPull Requestのおかげで英語に対応する事が出来ました。

## milk add -b オプションでブランチ指定
[milk add で git からコードを取得するとき，ブランチ名も指定したい · Issue #51](https://github.com/ongaeshi/milkode/issues/51)

[@niku](https://github.com/niku)さんの要望に対応しました。

## gmilkの出力モードにSJISを追加

```
$ gmilk -o sjis aaa
```

こちらもリクエストを頂き対応しました。

出力時の文字コードを明示的に指定出来るようになりました。秀丸やWindowsのEmacsなどで文字化けする場合に使ってみて下さい。

## リリースノート
* milk web
  * Add Filtering feature
    * Package
    * Suffix
    * Directory
    * Delete result_refinement
  * Support I18N (thanks takahashim)
  * Bugfix
    * Had forgotten Regexp.escape

* milk
  * Change Groonga::Schema table.text(“suffix”) -> table.string(“suffix”)
    * Need 'milk rebuild --all'
  * \#51 Add ‘milk add -b branch_name’ option (thanks niku)
  * Arrow 'https://.….git' to Util#git_url?
  * Add 'milk add --from-file LIST'

* gmilk
  * Add ‘gmilk -o’ option (Specify output encode)

## 関連記事
- [Milkode1.3リリース - GitHubボタンを追加、キーワードのマッチ箇所をハイライト](http://ongaeshi.hatenablog.com/entry/milkode-1.3)
- [Milkode 1.2 リリース : ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート](http://ongaeshi.hatenablog.com/entry/milkode-1.2)
- [Milkode 1.1 リリース : 待望の相対URLに対応、gmilkの高速化 ](http://ongaeshi.hatenablog.com/entry/milkode-1.1)
- [Rubyで特定のコマンドが存在するかを調べる方法](http://ongaeshi.hatenablog.com/entry/2013/06/18/144256)
- [Milkodeをgrepと組み合わせたら検索速度が40倍速くなった](http://ongaeshi.hatenablog.com/entry/milkode-and-grep)





