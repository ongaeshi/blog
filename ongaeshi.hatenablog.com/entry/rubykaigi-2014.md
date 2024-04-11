---
Title: ' RubyKaigi2014にいってきました(LTもしました)'
Category:
- ruby
- mruby
- rubykaigi
Date: 2014-09-21T19:41:57+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubykaigi-2014
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815733338321
---

はじめての参加だったのですがとても楽しかったです。
いいたいことを全部書くと長くなるので思いついたことから書いていこうと思います。
多分抜けがいっぱいあります。

## すたーと１日目

- ささださんの話でRubyの現状と次の課題がすごくよく分かった
- シンボルはGC対象にならないの知らなかった(そして2.2からGC対象になる)
- なぜGVL(Giant VM Lock)があると並列処理が遅くなるのかがなんとなく分かった
  - 専用ハードウェアやGoによるRubyVMの実装などでチャレンジしている人がいる
- Rubyにパターンマッチングが欲しい
  - という人が結構いた
- 動いているrubyプログラムのどの行がいちばんメモリを消費しているのか知りたい
  - [ko1/allocation_tracer](https://github.com/ko1/allocation_tracer) が便利
- コミットログを書くときに重視していることは？
  - なんで直したかは書いて欲しい
- コンパイラ分からなくてもコントリビュート出来る部分はありますか
  - 実はMacのメンテナはいない
- Ruby Summer of Code みたいな
  - Ruby Association で年三回やってます
- 2.2.0 preview をビルドして下さい
  - 自分のプロジェクトなどで実行やテストが動くか試して欲しい
- .notメソッドが欲しい
  - `true.!`がすでにある (！)
- パーティで須藤さんが色々な人に紹介してくれたのでぼっちにならずにすんだ

## どとうの２日目

- まつもとさんのSoft Typingの話、超面白かった
- Pixivの6割が海外ユーザーと聞いてびっくりした
- RubyGems&Bunlderを高速に動作させるために大改造をしていた
  - たしかに少し前からはやくなった気がする
  - 東京にミラーサーバーがあるらしい
- Rubyスクリプトでレーベンシュタイン距離を求めるスライドがすごく分かりやすかった
  - [schneems/going_the_distance](https://github.com/schneems/going_the_distance)
  - レーベンシュタイン距離はgitのスペルミスを教えてくれる機能とかで使われてるやつ
  - 最新のRailsにも入ったらしい
- ブラジルでmrubyを組み込んだPOSが動いてるらしい
- 新しいコードを100台以上あるサーバーにまとめてコピーするのが大変
  - Capistranoが何するgemなのか分かった
  - [sorah/mamiya](https://github.com/sorah/mamiya)
  - ゴシッププロトコル
- mod_mruby から [cgroups - Wikipedia](http://ja.wikipedia.org/wiki/Cgroups) を制御出来る
  - リソース制御に便利
- ServerEngineが高機能ぽくてよさそう
- Smalrubyのブロックを作ったらソースコードも編集されてソースコードも触れるのよさそう
- LT(ofrubyについて)
  - 緊張した(あんまりしゃべってた時の記憶がない)
  - 後で動画みたら(一応)ちゃんとしゃべってた
- 自分が書いたRailsチュートリアルのブログをRailsチュートリアルを作っている方が見てくれていて嬉しかった
- 夕飯のスープカレーが美味しかった
  - 東京グールが最終回と聞いてヤングジャンプを買ってホテルに戻る
    - 次は埼玉グールか神奈川グールではないだろうか(わりと真面目)
- 疲れがたまってきた

## らすと３日目

- おはようRubyは積極的にRailsを作っている企業を知ることが出来た
- メタプログラミングの話が分かりやすかった
  - 最初はOOPで作る
  - 問題をメタな観点からとらえ直す
  - 出来るだけ薄いラッパーで済ませる(そういう役割りのオブジェクトを生成して返すなど)
- 人のソースコードリーディングを見てるの楽しい
  - 関数へのジャンプは何使っていたのだろう？
- RubyからNES用のCソースコード吐くのすごかった
  - Ripper強力 - [class Ripper](http://docs.ruby-lang.org/ja/2.1.0/class/Ripper.html)
- シングルページWebアプリというのが流行っている
- [アンダースタンディング コンピュテーション](http://www.oreilly.co.jp/books/9784873116976/)を買った
  - 著者のささださんと、まえがきのまつもとさんにサインもらえた！
- [Ruby Under a Microscope - Pat Shaughnessy](http://patshaughnessy.net/ruby-under-a-microscope) がよさげ
- drubyでGoのチャネルエミュレーションしてた
  - Rubyの並列処理の問題にdrubyを上手く使えないだろうか
- Glue, Embed は普段意識せずに使っていたので分けて考えたり話す時に便利そう
- @tmmの話はすさまじい量の改善でGitHubのパフォーマンスが上がっていくのを見て圧倒された
  - プロファイル用のツールも使い捨てにせず徹底的に作り込んでいた
  - 作り込まれたものを見るのは楽しい、そして世界共通
- スタッフ紹介
  - すごい人数だった、規模が大きいなぁ
  - 2015の開催日はまだ確定していない(もう少し待ってね)

以下、いくつか印象に残った部分をもう少し。

## Building the Ruby interpreter -- What is easy and what is difficult?
ささださんの1日目のキーノート

- RGenGC (2.1)
  - Rubyのための世代別GC
  - Write BarrierはRubyに適用するのが難しかった
    - ので、別のアルゴリズムを実装した(！)
  - [RGenGCの発表資料を読んだ - oupoの日記](http://d.hatena.ne.jp/oupo/20130605/1370434919)
- Incremental GC (2.2)
  - [Rubyist Magazine - YARV Maniacs 【第 12 回】 インクリメンタル GC の導入](http://magazine.rubyist.net/?0048-YARVManiacs)
- 並列実行出来るようにしたい
  - [EASY] 単純に実装するのは簡単
  - [Difficult] よいプログラム体験を提供するのが難しい
  - 実際にやった報告とかして欲しい
- いいたいこと
  - まだまだブルーオーシャンはいっぱいあるよ
- 質問がハイレベルですごかった

## Coming soon...
まつもとさんの2日目のキーノート、Ruby3.0について

- RubyConf 2001 からずっと未来の話ばかりしている
- False rate = 7 / 22 = 32%
  - 68%は言ったことが実現出来ている
- OSSコミュニティは鮫
  - そろそろ燃料投下しよう
- 次の10年のテーマ
  - Concurrency
  - JIT(LLVM?)
  - Static typing
- Static typing?
  - Type Annotation の提案
    - Feature #9999 Davide D'Agostino

```ruby
def connect(r -> Stream, c -> Client) -> Fiber
  ...
end
```

- Soft-typing
  - もっと簡単に
  - メソッド内でaが呼ばれるメソッドを調べる
  - 呼べない関数があるオブジェクトを渡したらコンパイル時に警告を出す
    - 実行されなくても検出出来る
  - メタプログラミング全開のコードの場合は「警告を出さない」(今まで通り書くことは出来る)
  - Soft-typingが有効になる範囲「サブセット」を規定する
    - サブセットは何度も言っていたので大切な概念なのだと感じている

```ruby
a = 1 # type of a is Integer
```

```ruby
def foo(a)
  print a.to_int
end

foo(1)   # OK: 1 has to_int
foo("a") # NG: "a" does not have to_int
```

## LT: ofruby - Ruby running on the iPhone
[スライド](http://ongaeshi.me/slide/ofruby-rubykaigi-2014/#/)

- あらかじめ動画を作っておいてよかった
  - 5分だと実演は無理
  - 作業時間のうち動画作成にかなりの時間を費やしたけどそれだけの価値はあった
  - 時間が足りない時はスライドの作業を削った方がいいと思うくらい
- 終わった後にたくさんの人が感想や意見を言ってくれた
  - しゃべった甲斐があった
  - あるデザイナの方がLTの後にすぐダウンロードしてくれて、1時間位でタッチで模様の色が変わるデモを作って見せてくれた
    - こんなに早く作れるんだ！と驚いた。とても嬉しかった
  - Twitterのエゴサーチもとても参考になった(今も見ているので何かある人は"ofruby"付けてつぶやいてくれたらありがたいです)
- 課題も大分見えてきた
  - 何作っていいか分からない、というのが多かった
    - サンプルを同梱してコード読んだり、コピペして改造出来る仕組みを作ろう
    - 会場で受けが良かったのもTouch Fallのボールの数をリアルタイムで書き換える所だった
  - 遊べるサンプルが欲しい
    - 横スクロールアクションみたいなのがあればよい
    - ゲームじゃなくても何か使えればいいんだと思ってる
    - 例えば電光掲示板とか
  - コードをPCとやりとりしたい
    - 将来的には何か用意したい
    - 今は[Pastebot — Tapbots](http://tapbots.com/software/pastebot/)使っています
  - コードをシェアしたい
    - 将来的には何か用意したい
    - 今はPCに一旦コピーしてからやってます
  - バグを直す
    - iOS8だとちゃんと動かない？Xcode6をとりあえず試してみよう
  - ファイラーをまともに
    - ファイルを消したい
    - 名前や更新順でソートしたい
    - ファイル名で検索
  - p, print, puts デバッグがしたい
    - したいです
- LTの日のダウンロード数が前日と比べて(+783%)になりました

## まとめ
- 普段ネットでしか知らない人にたくさん会えて楽しかった
- RubyのConcurrencyとSoft Typingが面白そう
- RGenGCとIncremental GCの記事はそのうち読もう
- ofrubyも頑張ろう
- 来年もいきたいです
- スタッフのみなさまありがとうございました

