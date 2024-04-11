---
Title: ' Groongaを使うとドリルダウン検索は簡単です'
Category:
- ruby
- groonga
- rroonga
- web
Date: 2013-12-20T00:33:20+09:00
URL: https://ongaeshi.hatenablog.com/entry/groonga-drilldown
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815714779774
---

[全文検索エンジンGroonga Advent Calendar 2013の18日目](http://qiita.com/ongaeshi/items/35863925d153ebe034ae)の記事です。

[Groongaを囲む夕べ4](http://ongaeshi.hatenablog.com/entry/groonga04)で発表した[スライド](http://ongaeshi.me/slide/groonga04)にて、[Milkode](http://milkode.ongaeshi.me/)におけるドリルダウンの使い方を紹介してなかなか評判が良かったので記事にしてみます。

## ドリルダウンって？
- [ドリルダウンとは - IT用語辞典](http://e-words.jp/w/E38389E383AAE383ABE38380E382A6E383B3.html)
- [4.5. ドリルダウン — Groonga v3.1.0ドキュメント](http://groonga.org/ja/docs/tutorial/drilldown.html)

> データ集計や分析で用いる手法の一つで、集計範囲を一段階絞ってより詳細な集計を行うこと。

例えば内部でMilkodeを利用している[mruby Code Searchで検索](http://mruby-code-search.ongaeshi.me/home?query=mrb+self&shead=package)すると、マッチしたレコードの中からパッケージ名や拡張子名、ディレクトリ名でさらに絞り込むことが出来ます。これがドリルダウンです。

![drilldown-example](http://ongaeshi.me/slide/groonga04/images/slide-groonga04-01.png)

Amazonや価格.comといったサイトでもよく使われていますね。

## Groongaでドリルダウンを使うには？
GroongaのRubyバインディングであるRroongaで説明しますがGroongaでも基本は同じです。

下はRroongaのテーブルを検索して、その結果を"suffix"カラムでドリルダウンする例です。

```ruby
# Rroongaのテーブルを検索
result = table.select {|record| ... }

# 結果に対してgroupメソッドを呼ぶ
drilled = result.group("suffix")   # "suffix"カラムにドリルダウン!!

# 先頭を取得
r = drilled.first

# マッチ数とキーを取得
p [r.n_sub_records, r.key] #=> [356, "md"]
```

[groupメソッド](http://ranguba.org/rroonga/ja/Groonga/Table.html#group-instance_method)がドリルダウンの本体です。`[356, "md"]`は拡張子"md"のレコードが356個あることを示しています。

## Milkodeでの書き方

Milkodeでは[Util::drilldown](https://github.com/ongaeshi/milkode/blob/d8ca5658fe5c304a26ff6cb3ed8b870e8a29459d/lib/milkode/database/document_table.rb#L344-L347)というメソッドになりました。実際はワンライナーですが分解すると以下のようになります。

```ruby
# ドリルダウン結果を配列で返す(マッチ数、降順)
# column .. カラム名
# num .. 取得したい結果の数
def self.drilldown(result, column, num = nil)
  drilled = result.group(column).map {|record|
    [record.n_sub_records, record.key]
  }.sort_by {|a|
    a[0]
  }.reverse
  num ? drilled[0, num] : drilled
end
```

整理するとやっていることは

1. 検索結果をドリルダウン
2. n_sub_records(マッチ数), key(拡張子など) の配列を作る
3. マッチ数でソート
4. 降順に並べる
5. 必要な数だけ抜き出す

です。

## 引っかかったこと
Milkodeの場合ですが、

- 拡張子(suffix)カラムがtextカラムだったためドリルダウン対象に出来なかった。
- ドリルダウンしたかったのでやむなく1.4でstringカラムに変更
- データベースの再生成(milk rebuild --all)が必要になってしまった。

将来的にドリルダウンしたいものはカラム種類に気を付けましょう。

## まとめ
大切なことはこれだけです。

- Rroonga(Groonga)でドリルダウンしたい時は<b>selectで検索した後にgroup</b>を呼ぶ
- ドリルダウン結果に対して<b>n_sub_records</b>でマッチ数、<b>key</b>でドリルダウンしたカラムの値を取得
- ドリルダウンしたい要素のカラム種類には気をつける

## 参考文献
- [4.5. ドリルダウン — Groonga v3.1.0ドキュメント](http://groonga.org/ja/docs/tutorial/drilldown.html)
- [Rroongaリファレンスマニュアル](http://ranguba.org/rroonga/ja/)
- [Rroongaチュートリアル](http://ranguba.org/rroonga/ja/file.tutorial.html)

