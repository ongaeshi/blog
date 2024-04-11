---
Title: ' 動作検証やプラグイン開発に便利！Emacsを複数起動したり最小構成で起動する方法'
Category:
- emacs
Date: 2013-12-09T23:43:21+09:00
URL: https://ongaeshi.hatenablog.com/entry/emacs-options-q-l
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815714131710
---

これは[.emacs Advent Calendar 10日目](http://qiita.com/ongaeshi/items/32fddcd528643c2598a9)の記事です。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131208/20131208154942.gif" alt="f:id:tuto0621:20131208154942g:plain" title="f:id:tuto0621:20131208154942g:plain" class="hatena-fotolife" itemprop="image"></span></p>
<span style="color: #999999"><span style="font-size: 80%">
emacsのM-x shellから子emacsを起動</span></span>


## 複数個起動する
MacのEmacs.appだと意外に面倒なのです。

```
# Console 
$  emacs

# OSX(Emacs.app)
$ /Applications/Emacs.app/Contents/MacOS/Emacs

# 並列に起動
$ /Applications/Emacs.app/Contents/MacOS/Emacs &
```

Macの人はエイリアスを設定すると簡単になります。

```
alias emacsa='/Applications/Emacs.app/Contents/MacOS/Emacs'
```

## 最小構成で起動
`-Q` を付けると ~/.emacs.d/init.el 等の読み込みをせずに起動してくれます。

```
# Console
$  emacs -Q

# OSX(Emacs.app)
$ emacsa -Q &
```

### 応用: 安全なpackage-install
M-x package-install がエラーが出て失敗する場合などは-Qで起動するとよいです。

MELPAを使っている人は、起動後にパッケージを追加するのを忘れずに。

```lisp
(require 'package)
(add-to-list 'package-archives '("melpa" . "http://melpa.milkbox.net/packages/") t)
```


## 特定の.elだけを読み込んで起動
`-l`を使います。例えばマイナーモードを開発する時など、テストしたいelispだけを読み込んで起動します。

```
# Console
$  emacs -Q -l hoge-minor-mode.el

# OSX(Emacs.app)
$ emacsa -Q -l hoge-minor-mode.el &
```

## Special Thanks
Twitterで色々と教えてもらいました。インターネットって素晴らしいですね。

- @shohex
- @clothoid
