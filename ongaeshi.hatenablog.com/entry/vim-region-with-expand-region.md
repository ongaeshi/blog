---
Title: ' Emacsでexpand-regionをvim-region中に使えるようにしたら便利になった'
Category:
- emacs
Date: 2014-02-23T23:19:45+09:00
URL: https://ongaeshi.hatenablog.com/entry/vim-region-with-expand-region
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815718905992
---

[前回](http://ongaeshi.hatenablog.com/entry/emacs-vim-region)からバグ修正したりしながら地味にバージョンをアップをしています。

ふと思いついて[magnars/expand-region.el](https://github.com/magnars/expand-region.el)をvim-region中に"+","-"で使えるようにしたらなかなかにいい感じになったのでご紹介です。expand-regionについては[この辺](http://d.hatena.ne.jp/syohex/20120117/1326814127)が詳しいです。

## gitアニメと解説

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140223/20140223231014.gif" alt="f:id:tuto0621:20140223231014g:plain" title="f:id:tuto0621:20140223231014g:plain" class="hatena-fotolife" itemprop="image"></span></p>

1. `C-@`でvim-region-mode開始
1. `+`でexpand-region
1. もう一回`+`で関数全体を選択
1. `k`で上方向に選択範囲を移動、3回程実行してコメントと関数全体を選択
1. `d`で切り取り

## 素のexpand-regionとの比較

単体でexpand-regionを使うのと比べて何が便利なの？という話ですがexpand-regionって確かに便利なのですが「そこにもう少し選択範囲の調整を入れたい」ってことが多くて、例えば上の関数定義を選択した時もexpand-regionだけだと上のコメントまで選択出来なかったりします。

で、vim-regionとexpand-regionを組み合わせるとその後の微調整がとても簡単で、選択範囲を上下に広げたり(j,k)、左右(h,l)、単語(w,b)とかして最後に、コピーや切り取り(c, d) するのがコントロールキーを押さずに出来るので簡単＆軽快＆手首の負担軽減になります。


興味がある人は是非使ってみて下さい。

    M-x package-install vim-region



