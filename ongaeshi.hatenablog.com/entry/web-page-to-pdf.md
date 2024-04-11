---
Title: お気に入りのWebページをpdfに変換し、Honyomiに登録して検索出来るようにする
Category:
- book
- 本
- honyomi
Date: 2014-05-16T10:00:00+09:00
URL: https://ongaeshi.hatenablog.com/entry/web-page-to-pdf
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815724254321
---

毎日大量のWebページを眺めていると、時折何度も読み返したくなる素晴らしいページに出会うことがあります。

そのような時にブラウザの印刷機能を使ってpdfに変換し、[Honyomi
](https://github.com/ongaeshi/honyomi) に登録すると後々キーワードを使って読み返せるので便利です。

よい文章は何度も読み返して自分の血肉にしましょう。

## pdf に変換する

私は[ビジネスの悩みを解決するPDFドリル：Webページのスタイルを崩さずにPDF保存する裏技3選](http://bizmakoto.jp/bizid/articles/1309/24/news013.html)を参考にしましたが、`Webページ pdf`で検索すれば他にも色々出てくるはずです。

Firefoxなら[Print Edit](https://addons.mozilla.org/ja/firefox/addon/print-edit/)を使って不要な部分を取り除くことも出来ます。

## Honyomi に追加する
生成したpdfは

```
$ honyomi add ~/Books/xxxx.pdf
```

で追加しておきましょう。これで他の本と一緒に検索することが出来るようになります。

Honyomiのインストールや詳しい使い方は[こちら](http://ongaeshi.hatenablog.com/entry/honyomi-init)をどうぞ。

## おすすめは？
例えば[正確な文章の書き方](http://www.mew.org/~kazu/doc/japanese.html)や[作家とリヴァイアサン](http://www.geocities.jp/mickindex/orwell/orwell_WaL_jp.html)などがおすすめです。









