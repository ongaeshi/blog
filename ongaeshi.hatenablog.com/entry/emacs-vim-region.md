---
Title: 実践Vimを読んでEmacsの範囲選択をVimっぽく操作出来るvim-region.elを作りました
Date: 2013-12-24T00:14:30+09:00
URL: https://ongaeshi.hatenablog.com/entry/emacs-vim-region
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815715011264
---

[実践Vim](http://ascii.asciimw.jp/books/books/detail/978-4-04-891659-2.shtml)を読んで憧れて作ってみました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131224/20131224001122.png" alt="f:id:tuto0621:20131224001122p:plain" title="f:id:tuto0621:20131224001122p:plain" class="hatena-fotolife" itemprop="image"></span></p>

[ongaeshi/emacs-vim-region](https://github.com/ongaeshi/emacs-vim-region/)

範囲選択をvimっぽい w/b(単語), d(カット), y(コピー) で操作出来ます。

## インストール
```
M-x package-install vim-region
```

*~/.emacs.d/init.el* に以下の設定を追加して下さい。"C-@"は好きなキーを指定して下さい。勇気のある人は"C-SPC"を上書きしてもいいです。

```
(require 'vim-region)
(global-set-key (kbd "C-@") 'vim-region-mode)
```

[emacs-brave](https://github.com/ongaeshi/emacs-brave)には組み込み済みです。

## 使い方
"C-@"を押すとvimのように範囲選択出来ます。コピーやカット、vim-region以外のコマンドを実行すると自動で抜けます。

以下が実際に使えるコマンド一覧です。

```
"l"  forward-char
"j"  next-line
"k"  previous-line
"h"  backward-char

"a", "0"  move-beginning-of-line
"e", "$"  move-end-of-line

"y"  vim-region-copy
"d"  vim-region-kill
"p"  vim-region-yank
"c"  vim-region-copy
"x"  vim-region-delete-char

"z"  exchange-point-and-mark
"v"  vim-region-toggle-mark
"q"  vim-region-toggle-eternal

"w"  forward-word
"b"  backward-word

"s"  forward-sexp
"S"  backward-sexp

"t"  vim-region-mark-symbol
"r"  vim-region-query-replace

"m"  forward-paragraph
"M"  backward-paragraph

"g"  beginning-of-buffer
"G"  end-of-buffer

"O"  mark-whole-buffer

"C-f"  vim-region-scroll-up
"C-b"  vim-region-scroll-down

"/"  isearch-forward
"n"  isearch-repeat-forward
"?"  isearch-backward
"N"  isearch-repeat-backward

"f"  vim-region-forward-to-char
";"  vim-region-forward-last-char
"F"  vim-region-backward-to-char
","  vim-region-backward-last-char

"u"  undo
```

"s/S"でS式単位の選択、"r"で置換、"t"でカーソル上のシンボル選択、などもおすすめです。

## 効能
範囲選択時の左手小指の負担が軽減されます。
