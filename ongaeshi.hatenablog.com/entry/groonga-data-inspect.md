---
Title: ' groonga-databse-inspectを使ってMilkodeのデータ使用量を分析する'
Category:
- groonga
- milkode
Date: 2013-12-06T01:27:26+09:00
URL: https://ongaeshi.hatenablog.com/entry/groonga-data-inspect
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815713928417
---

この記事は[Groonga Advent Calendar 2013の6日目](http://qiita.com/ongaeshi/items/5a1f4c238681bb18f06a)です。

## これは何？
[[groonga-dev,01926] Rroonga 3.1.0](http://sourceforge.jp/projects/groonga/lists/archive/dev/2013-November/001928.html)
> 今回のリリースではgroonga-database-inspectというコマンドを追
加しています。このコマンドはデータベースの詳細を表示します。

Rroonga3.1.0から`groonga-database-inspect`というコマンドが追加されました。
Groongaの夕べ4の懇親会でも

- (質問者様) Milkodeって大体容量どれ位使うの？
- (私) え？(ちゃんと測った事無いぞー)

ということがあったので、しっかりと答えられるようになりたいです。

※ 先に結論を書いてしまうと登録したいパッケージの2.5倍位でした。どうやって調べたかを今から説明します。

## インストール

Rroonga3.1.0を入れると自動で付いてきます。

```
$ gem install rroonga
rroonga (3.1.0)
```

## 使い方
Milkodeのデータベースサイズを測ってみます。
データベース位置はデフォルトで`~/.milkode/db/milkode.db`です。

テストに使ったのは私が個人的に使用しているMilkodeのデータベース(171 packages, 145744 files) です。

```
$ groonga-database-inspect ~/.milkode/db/milkode.db
Database
  Path:             </Users/ongaeshi/.milkode/db/milkode.db>
  Total disk usage: 3.521GiB
  Disk usage:       29.266MiB (0.812%)
  N records:        528912
  N tables:         3
  N columns:        17
  Plugins:
    None
  Tables:
    documents:
      ID:               256
      Type:             hash
      Key type:         ShortText
      Tokenizer:        (no tokenizer)
      Normalizer:       (no normalizer)
      Path:             </Users/ongaeshi/.milkode/db/milkode.db.0000100>
      Total disk usage: 1.349GiB (38.308%)
      Disk usage:       28.062MiB (0.778%)
      N records:        145744
      N columns:        6
      Columns:
        content:
          ID:         260
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000104>
          Disk usage: 1.199GiB (34.062%)
        package:
          ID:         258
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000102>
          Disk usage: 16.262MiB (0.451%)
        path:
          ID:         257
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000101>
          Disk usage: 36.262MiB (1.006%)
        restpath:
          ID:         259
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000103>
          Disk usage: 44.262MiB (1.227%)
        suffix:
          ID:         262
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000106>
          Disk usage: 24.262MiB (0.673%)
        timestamp:
          ID:         261
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000105>
          Disk usage: 4.004MiB (0.111%)
    packages:
      ID:               269
      Type:             hash
      Key type:         ShortText
      Tokenizer:        (no tokenizer)
      Normalizer:       (no normalizer)
      Path:             </Users/ongaeshi/.milkode/db/milkode.db.000010D>
      Total disk usage: 68.602MiB (1.902%)
      Disk usage:       16.062MiB (0.445%)
      N records:        171
      N columns:        6
      Columns:
        addtime:
          ID:         272
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000110>
          Disk usage: 4.004MiB (0.111%)
        directory:
          ID:         271
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.000010F>
          Disk usage: 20.262MiB (0.562%)
        favtime:
          ID:         275
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000113>
          Disk usage: 4.004MiB (0.111%)
        name:
          ID:         270
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.000010E>
          Disk usage: 16.262MiB (0.451%)
        updatetime:
          ID:         273
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000111>
          Disk usage: 4.004MiB (0.111%)
        viewtime:
          ID:         274
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000112>
          Disk usage: 4.004MiB (0.111%)
    terms:
      ID:               263
      Type:             patricia trie
      Key type:         ShortText
      Tokenizer:        TokenBigramSplitSymbolAlphaDigit
      Normalizer:       NormalizerAuto
      Path:             </Users/ongaeshi/.milkode/db/milkode.db.0000107>
      Total disk usage: 2.077GiB (58.978%)
      Disk usage:       12.047MiB (0.334%)
      N records:        382997
      N columns:        5
      Columns:
        documents_content:
          ID:         267
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.000010B>
          Disk usage: 1.834GiB (52.074%)
        documents_package:
          ID:         265
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000109>
          Disk usage: 55.789MiB (1.547%)
        documents_path:
          ID:         264
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.0000108>
          Disk usage: 93.789MiB (2.601%)
        documents_restpath:
          ID:         266
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.000010A>
          Disk usage: 62.289MiB (1.727%)
        documents_suffix:
          ID:         268
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/.milkode/db/milkode.db.000010C>
          Disk usage: 25.039MiB (0.694%)
```

3.5GB, 528912 records, 3 tables, 17 columns ありました。

documents (ファイル情報), packages (パッケージ情報), terms (転置インデックス) の3つのテーブルがあり、それぞれ

- documents 1.349GiB (38.308%)
- packages  68.602MiB (1.902%)
- terms     2.077GiB (58.978%)

でした。ざっくりではありますが、データベースは

```
登録するパッケージのファイル容量 × 2.5
```

位になっているようです。

## 空のデータベースと比較
もう少し遊んでみます。空のMilkodeデータベースを作って値を比較してみます。

```
$ milk init ~/tmp/milkode_test/empty
create     : milkode_test/empty/milkode.yaml
create     : /Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db created.
```

何も追加されていない状態だとデータベースサイズは29MB位のようです。

```
$ groonga-database-inspect ~/tmp/milkode_test/empty/db/milkode.db
Database
  Path:             </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db>
  Total disk usage: 29.984MiB
  Disk usage:       21.266MiB (70.922%)
  N records:        0
  N tables:         3
  N columns:        17
  Plugins:
    None
  Tables:
    documents:
      ID:               256
      Type:             hash
      Key type:         ShortText
      Tokenizer:        (no tokenizer)
      Normalizer:       (no normalizer)
      Path:             </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000100>
      Total disk usage: 1.375MiB (4.586%)
      Disk usage:       64.000KiB (0.208%)
      N records:        0
      N columns:        6
      Columns:
        content:
          ID:         260
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000104>
          Disk usage: 268.000KiB (0.873%)
        package:
          ID:         258
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000102>
          Disk usage: 268.000KiB (0.873%)
        path:
          ID:         257
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000101>
          Disk usage: 268.000KiB (0.873%)
        restpath:
          ID:         259
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000103>
          Disk usage: 268.000KiB (0.873%)
        suffix:
          ID:         262
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000106>
          Disk usage: 268.000KiB (0.873%)
        timestamp:
          ID:         261
          Type:       scalar
          Value type: documents
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000105>
          Disk usage: 4.000KiB (0.013%)
    packages:
      ID:               269
      Type:             hash
      Key type:         ShortText
      Tokenizer:        (no tokenizer)
      Normalizer:       (no normalizer)
      Path:             </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010D>
      Total disk usage: 616.000KiB (2.006%)
      Disk usage:       64.000KiB (0.208%)
      N records:        0
      N columns:        6
      Columns:
        addtime:
          ID:         272
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000110>
          Disk usage: 4.000KiB (0.013%)
        directory:
          ID:         271
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010F>
          Disk usage: 268.000KiB (0.873%)
        favtime:
          ID:         275
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000113>
          Disk usage: 4.000KiB (0.013%)
        name:
          ID:         270
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010E>
          Disk usage: 268.000KiB (0.873%)
        updatetime:
          ID:         273
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000111>
          Disk usage: 4.000KiB (0.013%)
        viewtime:
          ID:         274
          Type:       scalar
          Value type: packages
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000112>
          Disk usage: 4.000KiB (0.013%)
    terms:
      ID:               263
      Type:             patricia trie
      Key type:         ShortText
      Tokenizer:        TokenBigramSplitSymbolAlphaDigit
      Normalizer:       NormalizerAuto
      Path:             </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000107>
      Total disk usage: 6.742MiB (22.486%)
      Disk usage:       4.047MiB (13.497%)
      N records:        0
      N columns:        5
      Columns:
        documents_content:
          ID:         267
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010B>
          Disk usage: 552.000KiB (1.798%)
        documents_package:
          ID:         265
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000109>
          Disk usage: 552.000KiB (1.798%)
        documents_path:
          ID:         264
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.0000108>
          Disk usage: 552.000KiB (1.798%)
        documents_restpath:
          ID:         266
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010A>
          Disk usage: 552.000KiB (1.798%)
        documents_suffix:
          ID:         268
          Type:       index
          Value type: terms
          Path:       </Users/ongaeshi/tmp/milkode_test/empty/db/milkode.db.000010C>
          Disk usage: 552.000KiB (1.798%)
```

## Groongaデータベースをダンプ、レストアしたい
`groonga-database-inspect`を使えばデータサイズを解析出来るようになりましたが、バックアップや巻き戻しといったことは出来ないのでしょうか？Rroongaインストール時に一緒についてくる`grndump`というツールを使うと可能なようです。

詳しくは以下の記事をどうぞ。

- [groongaのデータベースをダンプ・リストアする方法 - Qiita](http://qiita.com/groonga/items/7642e9e22bcda4b4327f)

## まとめ
- Milkodeは`登録するパッケージのファイル容量 × 2.5`位でデータベースが作成可能っぽい
  - ざっくり分析なのでもう少しサンプルを取ってみたい(他にサンプルとった方がいましたらネットに書いてくれたら嬉しい)
- `groonga-database-inspect`便利、Rroonga3.1.0についてくる
- `grndump`を使えばGroongaデータベースのダンプ、レストアも出来る

この辺、自前でデータ管理していると貧弱になりがちな所なので、せっかくGroongaを使っているなら積極的に活用していきたいです。

