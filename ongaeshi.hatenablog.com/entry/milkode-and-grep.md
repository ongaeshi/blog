---
Title: Milkodeをgrepと組み合わせたら検索速度が40倍速くなった
Category:
- groonga
- milkode
- ruby
Date: 2013-06-11T17:18:27+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-and-grep
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318754537670
---

ファイル数が多いと[gmilk](http://milkode.ongaeshi.me/gmilk.html)が遅くなるという指摘があったので高速化しました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130611/20130611002855.jpg" alt="f:id:tuto0621:20130611002855j:plain" title="f:id:tuto0621:20130611002855j:plain" class="hatena-fotolife" itemprop="image"></span></p>


## その1. テスト、ソースコード数の多いパッケージを登録
テスト用にlinuxをパッケージに登録。

```
$ tar xzvf linux-3.10-rc4.tag.xz
$ cd linux-3.10-rc4
$ milk add .
package    : linux-3.10-rc4
result     : 1 packages, 42810 records, 42810 add. (22m 39.2s)
*milkode*  : 188 packages, 201999 records in /Users/ongaeshi/.milkode/db/milkode.db.
```

4万ファイル、1600万行です。データとしては申し分無いです。

```
$ cd linux-3.10-rc4
$ milk info
Name:      linux-3.10-rc4
Records:   42810
Breakdown: C:34005(79%), Makefile:1713(4%), Text:1707(3%), .S:1375(3%), other:1688(3%), unknown:2322(5%)
Linecount: 16916142
```

## その2. 古いバージョンで検索

Milkode1.0.0、キーワードは`int main`で検索してみます。

```
$ time gmilk int main | wc 
    8375   59821  717038

real	1m22.843s
user	1m0.299s
sys	0m1.965s
```

1分22秒かかりましたこれはいかんですね。

※ 最後のwcは出力にかかる時間を省力して純粋に検索にかかった時間を調べるため

## その3. マッチしたレコード数を調べる

`gmilk -m`でキーワードにマッチしたレコード数を調べることが出来ます。(1.0.2から)

```
$ gmilk int main -m | wc
    9027    9027  711003
```

`int main`を含む行を持つ可能性のファイルが9027個。これを一つずつ探索するのはスクリプトなのでオーバーヘッドはそこかなあと予想。**ファイルリストさえ作ったらそれを検索するのはgrepとかagに任せた方が速いのでは？**というアイデアを思いついたのでやってみることに。

## その4. シェルで頑張る
`-m`でファイルリストを生成、結果を`xargs grep`に渡して、さらにその結果を`grep`に渡します。(後半はよくあるgrepでAND検索のテクニックです)

```
time gmilk int main -m | xargs grep -n int | grep main | wc
real	0m2.962s
user	0m1.564s
sys	0m0.217s
```

**1m22s -> 2.96s** と大分速くなりました(約40倍！)。同じキーワードを何回も入力しないといけないのが面倒ですね。

## その5.  -eオプションの追加
gmilkに`-e grep`を付けると、その4でやったことを自動でやってくれるようにしました。

```
$ time gmilk int main -e grep | wc 
   19427  124715 2623891

real	0m3.062s
user	0m2.685s
sys	0m0.429s
```

`-e ag`も出来ます。意外にもgrepの方が速かったです。

```
$ time gmilk int main -e ag | wc 
   19427  124715 2623891

real	0m6.356s
user	0m5.584s
sys	0m0.784s
```

## その6. 自動で"-e grep"する
マッチするレコードの数が`500`を超えると自動で`-e grep`の検索に切り替えます。(xargsとgrepがある環境のみ。)

```
$ time gmilk int main | wc 
Number of records is large. Use auto external tool (gmilk -e grep)
   19427  124715 2623891

real	0m3.038s
user	0m2.662s
sys	0m0.448s
```

レコード数が遅くてもスクリプトで検索したい時(ちゃんと行内の単語だけでマッチさせたい時など)は`--no-external`オプションを使って下さい。

```
$ time gmilk int main --no-external | wc 
    8375   59821  717038
```

**気にしなくても勝手に速くなるよ！**ってことです。

## ダウンロード
速くなった[Milkode](http://milkode.ongaeshi.me) を是非一度お試し下さい。

```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.0.2)

<span style="font-size: 80%">※ [milk web](http://milkode.ongaeshi.me/milk-web.html)は検索結果を1ページに表示出来る数だけ少しずつ検索していくのでこの問題はそもそもありません</span>

## おまけ. gmilkのいい所
1. AND検索のたびにコマンドをパイプでつなげなくてよい(私的には[gren](http://gren.ongaeshi.me/)の頃から大切)

2. 検索ディレクトリを指定せずに検索出来る

```
$ cd /a/proj/lib/command/
# プロジェクト全体から検索したい・・
$ grep -r -i a_keyword ../../

# ディレクトリ変わったのでパスも書き換えないと・・
$ cd foo/bar
$ grep -r -i a_keyword ../../../..
```

これが個人的には結構面倒です。gmilkだとパッケージを認識するので常にパッケージ全体を検索します。
```
$ cd /a/proj/lib/command/
$ gmilk a_keyword

$ cd foo/bar
$ gmilk a_keyword
```

常に`gmilk キーワード1 キーワード2 …`で検索出来ることを大切にしています。


