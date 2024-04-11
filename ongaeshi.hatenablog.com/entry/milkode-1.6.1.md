---
Title: Milkode 1.6.1 をリリースしました
Category:
- milkode
- ruby
- groonga
Date: 2014-04-18T00:22:30+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.6.1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815722186430
---

1.5の時に告知するのを忘れていました。まとめてご連絡です。

- トップページをリニューアル、タイムラインスタイルに (1.5)
- 環境変数 GMILK_OPTIONS のサポート
- gmilk で Ctrl+C 押下時にスタックトレースを表示しないように
- 安定化

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.6.1)


## トップページをリニューアル、タイムラインスタイルに
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140418/20140418002105.png" alt="f:id:tuto0621:20140418002105p:plain" title="f:id:tuto0621:20140418002105p:plain" class="hatena-fotolife" itemprop="image"></span></p>

最近追加やアップデートされたパッケージを見つけやすくなりました。

## 環境変数 GMILK_OPTIONS のサポート
`GREP_OPTIONS`的なやつです。

```
$ export GMILK_OPTIONS="-o sjis"
```

で常にsjisで出力されるようになります。

## gmilk で Ctrl+C 押下時にスタックトレースを表示しないように
[gmilk で Ctrl+C 押下時のスタックトレースを表示しないで欲しい · Issue #61](https://github.com/ongaeshi/milkode/issues/61)

gmilk実行中にCtrl+Cを押しても変なスタックトレースが出ないようになりました。

## 安定化
色々直しました。

- [Permission deniedで中断してしまう · Issue #57](https://github.com/ongaeshi/milkode/issues/57)
- [検索結果が多すぎる場合 · Issue #58](https://github.com/ongaeshi/milkode/issues/58)
- [milk web -n でthinのArgumentErrorとなり起動できない。 · Issue #64](https://github.com/ongaeshi/milkode/issues/64)
- [Windows版でファイルのパスが長い(259文字+null)場合、エラーになる。 · Issue #65](https://github.com/ongaeshi/milkode/issues/65)


## リリースノート
* milk web
  * トップページをリニューアル
    * タイムラインスタイルに変更
* milk
  * 57 Use "rescue Error::EACCESS" instead of FileTest.readable? for Windows (thanks kazuna)
  * 58 Use whichr in Util.exist_command? (thanks kazuna)
  * 65 Add FileTest.readable? (thanks kazuna)
* gmilk
  * 61 Without displaying stacktrace when pressed Ctrl+C on gmilk (thanks msmhrt)
  * Support ENV['GMILK_OPTIONS']
* etc
  * 64 thin < 2.0.0 in Gemfile (thanks MaskedW)
  * Add TravisCI Button
  * README.rdoc -> README.md
  * Util::func -> Util.func
  * Rewrite summary and description

## おまけ
るびまに寄稿しました。

- [Rubyist Magazine - Ruby でソースコード検索エンジンの作り方 〜Milkode の内部実装解説〜](http://magazine.rubyist.net/?0046-Milkode)

## 関連記事
- [Milkode1.4リリース - ドリルダウンによるマウスクリックの絞り込み機能を追加！](http://ongaeshi.hatenablog.com/entry/milkode-1.4)
- [Milkode1.3リリース - GitHubボタンを追加、キーワードのマッチ箇所をハイライト](http://ongaeshi.hatenablog.com/entry/milkode-1.3)
- [Milkode 1.2 リリース : ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート](http://ongaeshi.hatenablog.com/entry/milkode-1.2)
- [Milkode 1.1 リリース : 待望の相対URLに対応、gmilkの高速化 ](http://ongaeshi.hatenablog.com/entry/milkode-1.1)





