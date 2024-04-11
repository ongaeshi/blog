---
Title: RubyとGo言語を組み合わせて高速なgrepを作りました
Category:
- ruby
- go
- groonga
- milkode
Date: 2014-06-09T10:24:56+09:00
URL: https://ongaeshi.hatenablog.com/entry/create-fast-grep-using-ruby-and-go
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815725675070
---

[Milkode 1.7](http://ongaeshi.hatenablog.com/entry/milkode-1.7)で新しく入った[Gomilk](https://github.com/ongaeshi/gomilk)の技術解説です。ここ数ヶ月Go言語の勉強をしていましたが、Rubyで書かれたMilkodeとのよい組み合わせを思いつき、一ヶ月ほどかけて作ってみました。

## Gmilkの問題
Milkodeには[Gmilk](http://milkode.ongaeshi.me/gmilk.html)というGrep感覚で使えるコマンドラインツールが付属しているのですがもう少し高速に検索したいという欲求がずっとありました。

Gmilkが遅い原因としては

* 関連するライブラリがたくさんあってアプリケーションの起動が遅い
* 検索候補のファイル一覧を回すループ処理が遅い

というのが主な理由でした。

これらの問題を解決するために新しいプログラムを書きました。
名前はGo言語で作るので<b>Gomilk</b>としました(偶然ゴロがよかった)。

## 作戦
以下のような作戦で高速化を図りました。

1. あらかじめWebアプリを立ち上げておく 
2. Gomilkを実行
3. Gomilkは引数から検索クエリを作ってhttp経由でWebアプリに渡す
4. Webアプリはもらったクエリーを元にGroongaで全文検索して検索候補のファイル一覧を返す
5. 受け取ったファイル一覧を元にGomilk側でファイル内容の検索を行う

実際の処理と照らし合わせると以下のようになります。

### 1. あらかじめWebアプリを立ち上げておく 
    $ milk web -g

### 2. Gomilkを実行
    $ gomilk test

### 3. Gomilkは引数から検索クエリを作りhttp通信経由でWebアプリに渡す
    GET http://127.0.0.1:9292/gomilk?dir=%2Fpath%2Fto%2Fdir&query=test

### 4. Webアプリはもらったクエリーを元にGroongaで全文検索して検索候補のファイル一覧を返す
```
/path/to/dir/Gemfile
/path/to/dir/app.rb
```

### 5. 受け取ったファイル一覧を元にGomilk側でファイル内容の検索を行う

```
`path/to/dir/Gemfile`の中から`test`を探す・・
`/path/to/dir/app.rb`の中から`test`を探す・・
```

主要ライブラリの初期化処理はあらかじめWebアプリ側で済ませておくことでアプリケーションの起動時間の短縮化につながります。

2, 3, 5に相当するhttp通信とファイル一覧を元にした検索処理はGo言語で書きます(ここがGomilk)。バイナリに変換出来るのでプログラムの基本速度自体が高速になり、並列処理を使って複数のファイルを同時に検索することも可能になります。ファイル一覧を回すループはずいぶんと高速になるはずです。

Groongaとの通信やwebアプリ自体の制御はすでにRubyで書かれているものを再利用出来るためGoで書き直す量を最小限に絞ることも出来ました。(割と重要)

## 結果
OSX 10.7.5, Core2 Duo 3.06 GHz, 8 GB RAM, 500GB HDD.

それぞれのルートディレクトリから"performance test"を検索しました。(例: `grep -r "performance test"`)

### Ruby-2.1.2 (4722 files)
|                   | 検索1回目(sec)| 2回目以降(sec)     |
|:------------------|---------------:|---------------:|
| grep 2.14 (-r)    |     11.510     |   0.191        |
| ag 0.22 pre       |     7.024      |   0.159        |
| Gmilk  1.7.0      |     10.783      |   2.198        |
| Gomilk 0.1        |      4.110    |   0.228        |

### linux-3.10-rc4 (45752 files)
|                   | 検索1回目(sec)| 2回目以降(sec)     |
|:------------------|---------------:|---------------:|
| grep 2.14 (-r)    |       40.135   |   1.747        |
| ag 0.22 pre       |       35.923   |   1.459        |
| Gmilk  1.7.0      |        ※3.196  |   2.315        |
| Gomilk 0.1        |       11.400   |   0.539        |

- (おそらく)ファイル内容がHDDのキャッシュに載っているかどうかで検索速度が変わるため、1回目と2回目以降で分けて計測しました
- HDDのキャッシュクリアはマシンを再起動すれば多分クリアされるだろう、という前提で行っています。
- ※について : よく理由が分からなかったのですが妙に早かったです・・。Rubyを検索した後にLinuxを検索してデータを取っていたためGroongaデータベースの内容が事前にキャッシュされ高速になったのかもしれません。(でもそれならGomilkももっと早くなっていいよなぁ)

特にキャッシュに載る前の最初の検索において、全てのファイルの中身を調べる必要のあるgrepやagと比べて、全文検索エンジンであらかじめ検索対象を絞り込めるGmilk, Gomilk の方が有利な印象です。

Gomilkはag等の高速なgrepと比べても安定してよいパフォーマンスを出せている事が分かります。<b>Gmilkと比べると圧倒的に高速</b>です。ファイル数が増えてくるとGroongaで事前に絞り込めるためGomilkの効率はさらに良くなります。

## milk web 側の実装
`milk web` に `-g` オプションを付けた時に `GET /gomilk` を受け取れるようにします。例えば<b>検索ディレクリ: /path/to/dir</b>、<b>検索文字列: test</b>の場合は以下のようなURLを受け付けます。

    GET http://127.0.0.1:9292/gomilk?dir=%2Fpath%2Fto%2Fdir&query=test

受け取ったWebアプリはディレクトリと検索文字列を使ってGroongaデータベースを検索します。検索結果のファイル一覧をテキスト形式で返します。

    /path/to/dir/Gemfile
    /path/to/dir/app.rb

後はGomilk側にお任せです。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140607/20140607152915.png" alt="f:id:tuto0621:20140607152915p:plain" title="f:id:tuto0621:20140607152915p:plain" class="hatena-fotolife" itemprop="image"></span></p>

<span style="font-size: 80%">GET /gmilk の結果例。検索候補となるファイル一覧が表示される。</span>

## Gomilkの実装
pt([monochromegane/the_platinum_searcher](https://github.com/monochromegane/the_platinum_searcher))のコードを大分参考にしました。というかptが無かったらGomilkを作る気にもならなかったと思います。枕に足を向けて寝られません。

私の場合はよく検索するコード群はMilkodeに登録してgomilkで検索し、それ以外はptを使って検索するように使い分けています。ptはWindows版があるのと日本語ファイルに強いのが嬉しい所です。

Gomilkが独自に実装した部分としては、ptがディレクトリ内を検索してファイルリストを作る部分を、http経由でmilk webと通信する処理に置き換えたことです。

## まとめ
RubyとGoの組み合わせが上手くいって満足する検索スピードを得ることが出来ました。

私自身は今後は主にGomilkを使っていきますがGmilkも引き続きメンテナンスを続ける予定です。Rubyだけで動かせるGmilkの手軽さはやはり便利です。またGomilkやGmilkの最大のメリットは<b>「いちいち検索先ディレクトリを指定しないで検索出来る」</b>ことだと思っていて、agやgrepが高速でもGomilkを使えない環境であれば、Gmilkを使う価値は充分にあると思っています。

既存のアプリケーションをWebアプリとしてあらかじめ立ち上げておく事で、全てをGo言語に置き換える必要が無くなる、というのは色々と使えるアプローチではないかと思いました。

前も似たようなことを書いた気がしますがGo言語とRubyのようなLL言語の相性はとてもよいと思っていて、まずはRubyでさくさくと作り、後で速度が必要になってきた部分をGo言語で置き換えていくとなかなかいいです。Webアプリ等のバックエンドを置き換える例はすでに色々な所で紹介されていますが、Gomilkの場合はフロントエンドを置き換える一つの例になったのではないでしょうか。

Gomilkは<b>「検索先ディレクトリの指定が不要で、かつgrepやagよりも高速に検索出来る」</b>私にとって理想のツールになりました、是非一度お試し下さい。まだ出来たばかりなので随時機能は追加していく予定です。

