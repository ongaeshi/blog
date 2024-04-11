---
Title: org-modeとopen-junk-fileで始める快適メモ生活
Category:
- emacs
- org
- diary
Date: 2014-12-10T00:35:33+09:00
URL: https://ongaeshi.hatenablog.com/entry/org-and-open-junk-memo-life
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450076599392
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141210/20141210004229.gif" alt="f:id:tuto0621:20141210004229g:plain" title="f:id:tuto0621:20141210004229g:plain" class="hatena-fotolife" itemprop="image"></span></p>

この記事は[Emacs Advent Calendar 2014](http://qiita.com/advent-calendar/2014/emacs)の10日目です。

org-modeというEmacsの高性能メモ機能があります。

- 見出しで`TAB`を押すことでON/OFF出来るアウトラインプロセッサ
- ソースコードをメモ内に書いてその場で実行(文芸的プログラミング！)
  - ソースコードはちゃんと色付けされる
- `|`で囲むと表組
- htmlやpdfで書き出し
- プレゼンテーション
- TODOリスト

など出来ないことは無いのではないか、そもそもプログラムが書けるものをアウトラインプロセッサと言ってしまっていいのか、という位に色々な機能が入っています。

が、高機能すぎてどうやって使っていいか分からないという人も多いのではないでしょうか？(私もそうでした)。

私はかなり限定的にorg-modeを使っています。基本はアウトラインプロセッサ程度にしか使いこなしていません。ですがそれでも充分に便利です。たまに気が向いた時に新しい機能を調べてちょっとずつ使える引き出しを増やしています。

今回は私なりのorg-modeの使い方をご紹介したいと思います。

## ルール
- 朝イチでEmacsを起動(以後開きっ放し)
- `C-x j`(M-x open-junk-file)で`~/Documents/junk/2014-12-10-memo.org`を作成
- 1日の作業ログは基本ここに書き足していく(下から上)

これだけです。私の場合はこの`xxxx-xx-xx-memo.org`をTiddlyWikiのジャーナルに貼付けてバックアップ代わりにしています。

参考: [自分専用のメモを作って簡単に検索出来るようにする - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/20120130/1327934216)

## インストール

open-junk-fileプラグインをインストールしておきます。

```
M-x package-install open-junk-file
```

org-modeは新しめのEmacsなら標準で入っているはずです。無い人や最新にしたい人はインストールして下さい。

```
M-x package-install org
```

私のelispの設定です。

```lisp
;; open-junk-file
(require 'open-junk-file)
(setq open-junk-file-format "~/Documents/junk/%Y-%m%d-%H%M%S.")
(global-set-key "\C-xj" 'open-junk-file)

;; org
(require 'org)
(require 'ob-C)
(require 'ob-ruby)

(setq org-directory "~/Documents/junk")
(setq org-agenda-files (list org-directory))

(setq org-src-fontify-natively t)

(defun my-org-confirm-babel-evaluate (lang body)
  (not (or (string= lang "ditaa")
           (string= lang "emacs-lisp")
           (string= lang "ruby")
           (string= lang "C")
           (string= lang "cpp")
           )))

(setq org-confirm-babel-evaluate 'my-org-confirm-babel-evaluate)
```

## メモの例
ある日のメモを書き出すとこんな感じになります。上に書き足していって一日の終わりに次の日のTODOを軽く整理して終わりです。

この日はTODOを消化しながら、

- SINATRA_RELOADERが動かない問題を修正
- OSXがMavericksなって動かなくなったMacPortsを最新に更新
- popplerをインストール
- rvmをアンインストール (rvm seppuku)
- Emacsをports経由で入れていたのを忘れていてEmacsが消える、再インストール

などをしていました。メモを書いておくとちゃんと思い出せるのが一番のメリットです。

```org
* 明日のTODO
- 日本語Emacs入れる(shellが文字化け)
- Milkodeの文字コード問題、Debianで再現(Docker使う)
- Milkodeのショートリリース
- Milkodeのリリースブログ
- Honyomi、ブックマーク設計開始

* OSX環境だと再現しない
* なんか微妙に残っているので一回消す
rvm seppuku

* ややこしいのでrvm切る
#+begin_src shell
$rvm switch system
Switching /Users/ongaeshi/.rvm => system
rvm not found in system, please install and run 'rvm reload'
#+end_src

* なんとEmacs消えた・・、そういえばportsからインストールしてた

* いい機会だから必要になったものだけ再インストールしていこうかな
61.08 GB

* myports.txt
https://trac.macports.org/wiki/Migration#ReinstallMacPorts

  asciidoc @8.5.3_0+darwin
  asciidoc @8.6.6_0+python27 (active) platform='darwin 
.
.

* テストを通す
先にpopplerのインストールが必要

* ファイル内容直接表示
[[http://blog.livedoor.jp/sasata299/archives/51816222.html][gitで特定のファイルの特定のリビジョンの内容が見たい - (ﾟ∀ﾟ)o彡 sasata299's blog]]

git show a3aa5e7:lib/honyomi/util.rb

* 時間測る
参考 : http://127.0.0.1:9292/home/milkode/lib/milkode/cdweb/app.rb?query=elapsed&shead=package#n168

p331ページ位のテキスト表示

before: 2.043136 sec
after: 0.221836 sec

* SINATRA_RELOADERが上手く動作しない

#+begin_src shell
SINATRA_RELOADER=1 ruby -I~/Documents/honyomi/lib ~/Documents/honyomi/bin/honyomi web
#+end_src

* TODO TODOリスト作成 [4/5]
- [X] Honyomi: tag_keys(text, options = {})  に置き換え - [[http://ranguba.org/rroonga/ja/Groonga/PatriciaTrie.html#tag_keys-instance_method][link]]
- [X] Honyomi: 時間はかる
- [X] Honyomi: テストを通す
- [X] Milkode: tag_keys(text, options = {})  に置き換え - [[http://ranguba.org/rroonga/ja/Groonga/PatriciaTrie.html#tag_keys-instance_method][link]] -> TODO追加
- [ ] Milkode: grenfiletest.rb:12:in `match': invalid byte sequence in UTF-8 (ArgumentError)
```

Emacs上では各見出しは`TAB`で折り畳めるので視認性もよいです。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20141210/20141210003420.png" alt="f:id:tuto0621:20141210003420p:plain" title="f:id:tuto0621:20141210003420p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## org-modeの基本的な使い方
### 見出し

```
* 行頭の`*`が見出しです。TABで折りたたみ出来ます。
** 小見出し(h2)
*** 小小見出し(h3)
```

### 箇条書き

```
- 箇条書きは`-`です。これもTABで折りたたみ可能
  - 小要素
    - 小小要素
```

### リンク

```
[[%url%][%text%]]

Emacs Advent Calendar 2014 へのリンク例。
[[http://qiita.com/advent-calendar/2014/emacs][Emacs Advent Calendar 2014 - Qiita]]
```

### 表組

`|`で要素を囲むと表組されます。`TAB`を押すと自動で整形(幅を合わせてくれる)されるのがとても素晴らしいです。

```
| TokenBigram                                 | ×        | ×        | ×        | ×        |
| TokenBigramSplitSymbol                      | ○        | ×        | ×        | ×        |
| TokenBigramSplitSymbolAlpha                 | ○        | ○        | ×        | ×        |
| TokenBigramSplitSymbolAlphaDigit            | ○        | ○        | ○        | ×        |
| TokenBigramIgnoreBlank                      | ×        | ×        | ×        | ○        |
| TokenBigramIgnoreBlankSplitSymbol           | ○        | ×        | ×        | ○        |
| TokenBigramIgnoreBlankSplitSymbolAlpha      | ○        | ○        | ×        | ○        |
| TokenBigramIgnoreBlankSplitSymbolAlphaDigit | ○        | ○        | ○        | ○        |

```

### チェックボックス

```
* チェックボックス [4/5]
- [X] AAA
- [X] BBB
- [X] CCC
- [X] DDD
- [ ] EEE
```

### TODO

`Shift + → OR ←`でTODOとDONEを切り替え出来ます。

```
* TODO やること
* DONE 終わった
```

### ソースコード

お気に入り機能。org-modeだとコードはちゃんとruby-modeで色付け
されます。`C-c C-c`でプログラムを実行することも出来ます。

```
#+begin_src ruby
1 + 2 * 100000
#+end_src

#+RESULTS:
: 200001
```

## おわりに

テキストファイルや*scratch*に日報や作業ログを書いている人はちょっと高機能なテキストモードとして試しに使ってみてはいかがでしょうか？駆け足でしたがよいorgライフを！

※ 私が紹介したのはorg沼のほんの入り口に過ぎません・・。
