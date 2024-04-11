---
Title: Milkode 1.8.9 リリース - キーボードショートカットがより便利に
Category:
- milkode
Date: 2016-07-18T00:03:19+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.8.9
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653812171406047662
---

ひそかに1.8.8もリリースしていたのですがブログ書いてなかったのでまとめて。

[リリースノート](https://github.com/ongaeshi/milkode/blob/master/HISTORY.ja.rdoc)

## 変更点
- ショートカットキーの s が押されると、ページトップにスクロール
- 検索ボックスにフォーカスが移ると、検索ボックスのキーワードをすべて選択状態にする
  - https://github.com/ongaeshi/milkode/pull/82
- 相対URLモードで起動していると、ショートカットキー S による新タブ検索が行えない問題を修正
  - https://github.com/ongaeshi/milkode/pull/83
- Fix LoadError of coderay/scanner
  - https://github.com/ongaeshi/milkode/pull/79
- Exit with 1 on error
  - https://github.com/ongaeshi/milkode/pull/81

特に"ショートカットキーの s が押されると、ページトップにスクロール"と"相対URLモードで起動していると、ショートカットキー S による新タブ検索が行えない問題を修正"のおかげでショートカットキー(milk webを起動して[ヘルプ]->[キーボードショートカット])が大分使いやすくなりました。ぜひお試しください。

## インストール
行指向のソースコード検索エンジンと検索アプリです。
数万オーダーのファイルから、目的のキーワードを含む1行を瞬時に検索することが可能です。

    $ gem install milkode

http://milkode.ongaeshi.me/

