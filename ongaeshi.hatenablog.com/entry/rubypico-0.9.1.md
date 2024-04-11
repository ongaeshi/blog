---
Title: RubyPico 0.9.1 リリース - ファイルマネージャーの強化、File、Dir
Category:
- rubypico
Date: 2016-11-06T21:47:15+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.9.1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687193242208
---

RubyPico 0.9.1 をリリースしました。ファイルマネージャーの強化、File、Dirが使えるようになりました。

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## ファイルマネージャーの強化
ディレクトリが作れるようになりました。実験的なプログラムを`/tmp`以下に置いたり、複雑なアプリケーションを1つのディレクリ以下にまとめておくことができるようになりました。他にも色々パワーアップしています。

- Editボタンを追加
- ディレトリの表示、作成
- 削除
- 移動
- 名前変更
- ソート種類の変更

[f:id:tuto0621:20161106214817p:plain]
[f:id:tuto0621:20161106214906p:plain]

## File, Dir
`File`、`Dir`ライブラリが使えるようになり、プログラム上でファイルを作ったり、実行時にファイルを読み込むことができるようになりました。

## 例: tango
複雑なアプリケーションの例として単語帳を作ってみました。

[ongaeshi/tango: Flashcard for RubyPico](https://github.com/ongaeshi/tango)

- 登録した単語を`word.json`以下に保存
- 次回起動時に`word.json`を読み込み
- 単語の追加、訳の追加、単語を削除したときは`word.json`を更新

[f:id:tuto0621:20161106214914j:plain]
