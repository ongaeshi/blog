---
Title: ' RubyとGrnMiniを使ってレストラン検索を作ってみた'
Category:
- groonga
- ruby
- grn_mini
Date: 2014-02-04T10:06:02+09:00
URL: https://ongaeshi.hatenablog.com/entry/grn-mini-restaurant
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815717696909
---

[Elasticsearchチュートリアル - 不可視点](http://code46.hatenablog.com/entry/2014/01/21/115620)で紹介されていたlivedoorグルメの研究用データセットと[GrnMini 0.4.0](http://ongaeshi.hatenablog.com/entry/grn-mini-4.0)を使って<b>Groongaベースの簡単なレストラン検索</b>を作ってみました。

ソースコードもとても短い(153行)ので簡単な検索エンジンを作ってみたい人は参考になるのではないかと思います。

## ソースコード

[grn_mini_samples/restaurants.rb](https://github.com/ongaeshi/grn_mini_samples/blob/master/restaurants.rb) 

## 使い方

1. http://blog.livedoor.jp/techblog/archives/65836960.html から研究用データをダウンロード
2. <i>ldgourmet.tar.gz</i> を展開
3. データベースの作成
4. 検索

### データベースの作成

展開した <i>ldgourmet.tar.gz</i> を指定して実行します。しばらく時間がかかります。

    $ ruby restaurants.rb /path/to/ldgourmet/

### 検索

キーワードにマッチする店名や地域を探します。

```
$ ruby restaurants.rb ラーメン 新宿
ラーメン二郎 - 新宿区西新宿7-5-51F
ラーメンむろや - 新宿区四谷4-25-10ダイアパレス御苑前1F
北海道ラーメン 新宿源 - 渋谷区代々木2-5-5新宿農協会館1F
ラーメン二郎 - 新宿区歌舞伎町1-19-3歌舞伎町振興組合ビル1F
・
・
```

`-r`でキーワードにマッチするレビューを探します。

```
$ ruby restaurants.rb -r ラーメン とんこつ さっぱり
--- イレブンフーズ - 品川区東品川1-34-23 ---
ー10席程度のみの小さなお店で、<<とんこつ>>醤油のような  背油醤油のような
ゲがどーんと乗っていて  普通の<<ラーメン>>でも全然満足。叉焼麺と大盛が
人はいません。  生玉葱の甘みと<<さっぱり>>感でもたれることなく食べられ
--- 麺処 中村屋 - 大和市下和田1207-1-C ---
ここの塩<<ラーメン>>はナンバー１です。超おいしかった！感動しました!!  神
油なら、六角家。<<さっぱり>>なら、支那そばや。<<  とんこつ>>は、私は苦手な
--- 麺の坊 砦 - 渋谷区神泉町20-23 ---
かもしれないですけど、ここの<<ラーメン>>は共通する本物の職人を感じます
歳。夜中に酔ったついでに〆の<<ラーメン>>を・・・。「いかん!!  いかん!!」
いながら誘惑に負けてやっぱり<<ラーメン>>屋へ・・・。気にしてる割に福岡
.
.
```
