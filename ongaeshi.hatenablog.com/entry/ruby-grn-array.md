---
Title: ' RubyでただのArrayだと思ってデータを追加したつもりなのに気がついたら全文検索出来ていた・・的なものを作った'
Category:
- ruby
- rroonga
- groonga
- web
Date: 2013-12-22T16:56:40+09:00
URL: https://ongaeshi.hatenablog.com/entry/ruby-grn-array
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815714901175
---

<span style="font-size: 200%">[続き](http://ongaeshi.hatenablog.com/entry/create-grn-mini)を書きました。</span>

[Ruby Advent Calendar 22日目の記事](http://qiita.com/ongaeshi/items/bd871638a255341ddd7a)です

![Rroonga-logo](http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131222/20131222154436.png?1387694685)

[Rroonga](http://ranguba.org/rroonga/ja/)というRubyで使える全文検索エンジンがあって愛用しているのですが、使う前の準備でカラム指定やデータ型を指定したり、全文検索のためのインデックステーブルを作るのが少し大変でした(大規模なアプリケーションの時はしっかり定義出来るので便利なのですが)。

普段使いで全文検索するために、実験的にRubyのArrayのように使えるようにしてみました。

## インストール
Rroongaを使うにはgemのインストールが必要です。他の全文検索エンジンと違って<b>それ以外のソフトウェアのインストールが不要</b>なのがいい所です。Windowsでも問題なく動きます。

```
$ gem install rroonga
```

今回書いたコードは以下にまとめてあります。

[ongaeshi/grn_array - GitHub](https://github.com/ongaeshi/grn_array)

```
$ git clone https://github.com/ongaeshi/grn_array.git
$ cd grn_array/
```

これで準備完了です。

## 基本的な使い方
データを追加して検索するまでのサンプルコードです。Rroongaと比べると割と簡単に使えるのではないかと思います。

```ruby
require_relative './grn_array'

# 配列の生成
array = GrnArray.new("db/simple.db")

# データの追加
if array.empty?
  array << {text:"aaaa", name: "a.txt"}
  array << {text:"BBBB", name: "b.txt"}
  array << {text:"cccc", name: "c.txt"}
end

# 検索(デフォルトカラムはtext)
results = array.select("bb OR cc")

＃ 結果を表示
results.each do |record|
  puts name: record.name, text: record.text
end
```

実行するとマッチしたレコードを返します。

```
$ ruby simple.rb 
{:name=>"b.txt", :text=>"BBBB"}
{:name=>"c.txt", :text=>"cccc"}
```

- `GrnArray#new(path)` で保存先のデータベース名を指定します。`db/simple.db*`がデータベース本体です。すでにデータベースが存在している時はそのデータベースを開きます。
- `GrnArray#<<` でデータを追加します。シンボルをキーにしたハッシュにして渡します。
- `GrnArray#select(query)` が検索です。どのようなコマンドが使えるかは[Groongaのクエリー構文](http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html)を見て下さい。`text:`で渡したデータがデフォルトカラムになります。
- 検索結果はIEnumerableなのでeachで回せます。受け取ったレコードはシンボルと同じ名前のメソッドでアクセス可能です。

## どれくらい速いの？
Twitter風の一行テキストを 100, 1000, 10000, 100000, 1000000 .. と増やしながら Array#grep と検索速度を比較してみます。

```ruby
require_relative './grn_array'
require 'benchmark'

GrnArray.tmpdb do |array|
  native_array = []
  
  texts = File.read('dummy/dummy1.txt').split

  TEST_TIMING = [100, 1000, 10000, 100000, 1000000]
  DATA_NUM    = TEST_TIMING[-1]
  test_index = 0

  DATA_NUM.times.each do |index|
    text = texts[rand(texts.size)]
    array << {text: text}
    native_array << text

    if (array.size == TEST_TIMING[test_index])
      puts "-- #{array.size} --"
      Benchmark.bm(16) do |x|
        x.report("GrnArray#select") { 100.times { array.select("しかし") } }
        x.report("Array#grep")      { 100.times { native_array.grep(/しかし/) } }
      end
      test_index += 1
    end
  end
end
```

GrnArray#tmpdbはテンポラリにデータベースを作ってブロックを抜けたら削除するもので、テストプログラムなどでいちいちデータベースを管理したくない時に便利です。
実行してみましょう。

```
$ ruby benchmark.rb
-- 100のデータを100回検索 --
                       user     system      total        real
GrnArray#select    0.020000   0.010000   0.030000 (  0.036112)
Array#grep         0.000000   0.000000   0.000000 (  0.006804)

-- 1000のデータを100回検索 --
                       user     system      total        real
GrnArray#select    0.020000   0.010000   0.030000 (  0.032740)
Array#grep         0.070000   0.010000   0.080000 (  0.067054)

-- 1万のデータを100回検索 --
                       user     system      total        real
GrnArray#select    0.040000   0.020000   0.060000 (  0.047148)
Array#grep         0.660000   0.000000   0.660000 (  0.680305)

-- 10万のデータを100回検索 --
                       user     system      total        real
GrnArray#select    0.180000   0.040000   0.220000 (  0.217387)
Array#grep         6.600000   0.030000   6.630000 (  6.674841)

-- 100万のデータを100回検索 --
                       user     system      total        real
GrnArray#select    1.520000   0.340000   1.860000 (  1.972344)
Array#grep        68.320000   0.390000  68.710000 ( 71.439266)
```

データ数が少ない時はそれほど変わりませんが、データ数が増えてくるとGrnArray#selectがどんどん高速になります。

![グラフ](http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131222/20131222153820_original.png?1387694423)

## スニペット
全文検索エンジンを使う際に、もう一つ知っておくと便利な機能があります。
<b>スニペットとかKWIC</b>というもので、キーワード周辺の文章を表示するための機能です。詳しくは[groongaをRackに載せて全文検索 - キーワード周辺の文章の表示](http://www.clear-code.com/blog/2009/7/31.html#.E3.82.AD.E3.83.BC.E3.83.AF.E3.83.BC.E3.83.89.E5.91.A8.E8.BE.BA.E3.81.AE.E6.96.87.E7.AB.A0.E3.81.AE.E8.A1.A8.E7.A4.BA)をどうぞ。

先ほどのようなTwitter風の短いテキストならマッチした全テキストを表示すればいいのですが、Google検索のように一つのレコードに長い文章が含まれている場合はマッチ周辺だけを表示した方が便利です。

GrnArrayでスニペットを使うには検索結果の`GrnArray::Result`に対して`Result#snippet_text(open_tag, close_tag)` を使います。生成したスニペットに対してマッチ周辺だけを表示したい要素を渡す(`snippet.execute(record.text)`)と、マッチ個所が指定されたデリミタで囲まれたものが配列として返されます。若干複雑なのでコードを見るのが一番分かりやすいと思います。

```ruby
require_relative './grn_array'

GrnArray.tmpdb do |array|
  # データを追加
  array << {name: 'dummy1.txt', text: File.read('dummy/dummy1.txt') }
  array << {name: 'dummy2.txt', text: File.read('dummy/dummy2.txt') }

  # 'けれども'で検索
  results = array.select('けれども')

  # スニペットの作成
  snippet = results.snippet_text('<<', '>>')

  results.each do |record|
    puts "--- #{record.name} ---"
    # スニペットを適用したものを検索結果として表示
    snippet.execute(record.text).each do |segment|
      puts segment.gsub("\n", "")
    end
  end
end
```

実行結果は以下のようになります。

```
$ ruby snippet.rb
--- dummy1.txt ---
いたくです<<けれども>>少しお肴連れないのますなた。<<けれども>>お笑いか自由
でも無論は最も人って来るです<<けれども>>、私には時間中まで私のご運動は
学習の最後をしがしまえのです<<けれども>>、何をいうて、その活動院という
--- dummy2.txt ---
日云いただ。そうかとトマトは<<けれども>>のそのそとらたませていいんが
の声からあんまりすまました。<<けれども>>むっとさきのようましたばこに「
」というばかが出るときたた。<<けれども>>こどもはすぐ頭をよろよろして
```

マッチ箇所が'<<','>>'で囲まれて表示されました(検索サイトっぽくなってきましたね)。HTMLを生成したい時は Result#snippt_htmlというのもあります。

## GrnArrayのソース
今回作ったGrnArray(grn_array.rb)は100行程度のRroongaの薄いラッパーです。Rroongaの簡単なサンプルコードとしてお使い下さい。

```ruby
# -*- coding: utf-8 -*-
require 'groonga'
require 'tmpdir'

class GrnArray
  include Enumerable

  def self.tmpdb
    Dir.mktmpdir do |dir|
      yield self.new(File.join(dir, "tmp.db"))
    end
  end

  def initialize(path)
    unless File.exist?(path)
      Groonga::Database.create(path: path)
    else
      Groonga::Database.open(path)
    end

    unless Groonga["Array"]
      @grn = Groonga::Array.create(name: "Array", persistent: true) 
      @terms = Groonga::PatriciaTrie.create(name: "Terms", key_normalize: true, default_tokenizer: "TokenBigramSplitSymbolAlphaDigit")
    else
      @grn = Groonga["Array"]
      @terms = Groonga["Terms"]
    end
  end

  def <<(value)
    if @grn.empty?
      value.each do |key, value|
        column = key.to_s
        @grn.define_column(column, "Text") # データ型は"Text"決めうち @todo valueの型種類を元に類推出来るはず
        @terms.define_index_column("array_#{column}", @grn, source: "Array.#{column}", with_position: true)
      end
    end
    
    @grn.add(value)
  end

  def select(query)
    Results.new(@grn.select(query, {default_column: "text"})) # textカラムを検索時のデフォルトカラムとする
  end

  def size
    @grn.size
  end

  def empty?
    size == 0
  end

  def each
    @grn.each do |record|
      yield record
    end
  end

  class Results
    attr_reader :grn
    include Enumerable

    def initialize(grn)
      @grn = grn
    end

    def each
      @grn.each do |r| 
        yield r
      end
    end

    def size
      @grn.size
    end

    def snippet(tags, options = nil)
      @grn.expression.snippet(tags, options)
    end

    def snippet_text(open_tag = '<<', close_tag = ">>")
      @grn.expression.snippet([[open_tag, close_tag]])
    end

    def snippet_html(open_tag = '<strong>', close_tag = "</strong>")
      @grn.expression.snippet([[open_tag, close_tag]], {html_escape: true})
    end
  end

  # その内...

  def []
  end

  def []=
  end


  def clear
  end
end
```

## つぶやき
- ダミーテキストには[すぐ使えるダミーテキスト - 日本語 Lorem ipsum](http://lipsum.sugutsukaeru.jp/)を使わせて頂きました
- GrnArray#select(query)の使い方は[8.10.1. クエリー構文](http://groonga.org/ja/docs/reference/grn_expr/query_syntax.html)を参考にして下さい
- [Rroongaリファレンスマニュアル](http://ranguba.org/rroonga/ja/)は、右上の[Class List], [Method List]を使うのがポイントです
  - 初めてRroongaを使う人は [ファイル名検索を高速化するためにRubyとGroonga（Rroonga）を使った話（Windows対応）](http://qiita.com/myokoym@github/items/55c07a30bafc18e3fdb4) も分かりやすいです。
- GrnArrayは現状テキストしか追加出来ないのですが、Rroonga自体は数値や位置情報を保持出来るので対応する余地はありそうです
  - 数値として登録するとあるカラムの値が100以下、みたいな検索が出来るようになります

