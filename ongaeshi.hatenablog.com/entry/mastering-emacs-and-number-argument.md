---
Title: Mastering Emacsのすすめと、意外と便利な数引数の話
Category:
- emacs
Date: 2015-12-22T00:07:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/mastering-emacs-and-number-argument
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347149412864
---

この記事は[Emacs Advent Calendar](http://qiita.com/advent-calendar/2015/emacs) 22日目の記事です。前日は[Emacs Lispでシェルコマンドを活用する](http://qiita.com/ShingoFukuyama/items/a4784189008ca99cdada)でした。

最近[Mastering Emacs](https://www.masteringemacs.org/)という本を読みました。Emacsの本を購入したのは久しぶりですがかなり面白かったです。洋書なので読むのが大変でしたが苦労して読む価値はあったと感じています。

すごいなぁと思うのがEmacsの最大の魅力の1つである「カスタマイズ」に関する話が<b>ほとんどない</b>ということです。序盤にほんの少しだけカスタマイズの方法が書いてありますが(しかもそれがM-x customizeの使い方だったりする)、Emacsの歴史、インストール、起動オプション、ヘルプの読み方、カーソルの移動、テキストの編集・・と続きます。内容の大半はEmacsの基本的な機能を深く知ることにフォーカスされているのです。[実践Vim](http://tatsu-zine.com/books/practical-vim)というVimの中核となるコンセプトを紹介した素晴らしい本がありますが本書はそのEmacs版だといってしまってもよいかも知れません。

今回はMastering Emacsの中でも特に面白かった数引数の話を紹介します。

## タイプ数を減らせ
すいません、ここに書いてあったことの意味を完全にはき違えていました(詳しくはコメントで指摘された通りです)。打ち消し線の部分は完全に間違いなので読み飛ばしてください・・。

<s>
「Chapter 5 The Theory of Editing」(チャプター5 編集のセオリー)でLinux作者Linusさんの有名な言葉が引用されていました。

> "An inﬁnite number of monkeys typing into GNU Emacs would never make a good program." - Linus Torvalds

「無駄なキータイプをEmacsに打ち込んでいる限りは良いプログラムが作られることはないだろう(意訳)」もっとひどいニュアンスで言っている気もしますがまぁこんな意味じゃないかと。Emacsにはタイプ数を減らすための機能がたくさん用意されているのでうまく使いこなしてプログラムに集中しましょうということだと(前向きに)理解しました。</s>

Emacsにはタイプ数を減らすための機能がたくさん用意されているのでうまく使いましょう。

作者のMickey Petersenも「Marking is unnecessary」(マーキングは不要です)と言っています。直接テキストを切り取る方法がある場合はそちらを使った方が マーク→選択→kill よりも2手省力できるということですね。

要はできるだけタイプ数を少なく目的の編集を達成する方が良い、そしてはEmacsはそれができるように設計されている、と言っているのだと思います。

## 数引数と組み合わせてさらに減らす
以下のような状況でfoo, barを削除することを考えます。(「□」はカーソル位置です。) 通常は行頭に戻って(C-a)、行削除(C-k)を繰り返したり、行全体削除(`C-S-<backspace>`)を3回押すような人が多いと思います。

```ruby
def foo
  foo□
  bar

  baz
end
```

しかし、3行消したいのは自明なので数引数を使います。 `C-3 C-S-<backspace>` です。

```ruby
def foo
□  baz
end
```

続いて書籍内の例を引用します。カーソル手前の文字列を削除したいと考えています。

```python
s = make_upper_case("hello, world!"□)
```

通常は文字列の先頭にカーソルを動かして削除と考えます。しかしマイナスの数引数を使うと1コマンドで削除できるのです。`C-M-- C-M-k`で後ろ方向にS式削除です。

```python
s = make_upper_case(□)
```

※ S式はEmacsが用意している構文単位で、大抵のメジャーモードでは括弧や文字列、単語の並びなどをまとめて1コマンドで処理することができます

他にもother-windowの逆スクロールをマイナス数引数で行うという例も紹介されていました。通常スクロールが`C-M-v`になり逆スクロールが`C-M-- C-M-v`になります。逆スクロールには`C-M-S-v`というキーバインドが専用に割り当てられているのですが、`C-M-v`と`C-M-S-v`を交互に繰り返すよりも`C-M-v`と`C-M-- C-M-v`を繰り返した方が不思議とテンポがいいのです(ぜひ実際にやって見てください)。

## 数引数はC-, M-, C-M- 全てに割り当てられている
実は数引数はEmacsの中に大量に割り当てられています。

- C-1..9,0,-
- M-1..9,0,-
- C-M-1..9,0,-

なんと`33種類のキーバインド`に割り当てられています。数引数として35を渡したい時は

- `C-3` `C-5` (数引数35)
- `M-3` `M-5` (数引数35)
- `C-M-3` `M-5` (数引数35)

どれも35を渡せます。なぜこのような配置になっているのでしょうか？

それは、どのモディファイア(Modifier, C-,M-,C-Mなどの他のキーと組み合わせて使うキーのことです)を使うコマンドでも同じテンポで数引数を入力するためなのです。例えばC-1..9,0,-のみに数引数の設定が渡されていたら、C-fやC-kといったC-から始まるコマンドはテンポよく入力できますが、M-fやC-M-kといったコマンドに数引数を渡す時に大きくテンポが狂います。

- `C-3` `C-5` `C-f` (Good)
- `C-3` `C-5` `M-f` (CからMにモディフイアを切り替える必要がある)
- `C-3` `C-5` `C-M-k` (CからC-Mにモディフイアを切り替える必要がある)

これでは手首が疲れてしまいます。そこでEmacsにはC-,M-,C-M-全てに数引数が設定されているのです。

- `C-3` `C-5` `C-f` (Good)
- `M-3` `M-5` `M-f` (Good)
- `C-M-3` `C-M-5` `C-M-k` (Good)

実行したいコマンドと同じモディファイアの数引数を使うことでテンポよく数引数を入力することができますね。

## まとめ: テンポの良いコマンド入力を意識しよう
書籍の中でも **tempo**(テンポ) という言葉が何度も出てきます。タイプ数の少ない、テンポの良い編集を行うことでコードそのものに集中できる時間が増えていきます。

このようなダイレクトな編集方法はVimの専売特許だとずっと思っていました。しかしC-(近距離),M-(中距離),C-M-(遠距離)に割り当てられた構文単位の編集コマンドと、数引数や負の引数を組み合わせることでEmacsでも十分にダイレクトな編集ができるのだと気が付かされました。Mastering Emacsに感謝しています。おかげで10年以上かけて上書きしてしまったキーバインドを外して[デフォルトに戻していくコミット](https://github.com/ongaeshi/.emacs.d/commits/master)が続いています・・。

Mastering Emacsには他にも徹底的なヘルプの使い方や今まで知らなかったたくさんの(標準添付された)便利コマンドなどがたくさん紹介されています。ぜひたくさんの人に読んでもらってこの感動をみなさんと共有したいです(そしてぜひとも日本語で読みたい)。内容について興味のあることがございましたら[@ongaeshi](https://twitter.com/ongaeshi)までお気軽にどうぞ、それでは。

[f:id:tuto0621:20151222000508j:plain]

[The Mastering Emacs Ebook](https://www.masteringemacs.org/order)
