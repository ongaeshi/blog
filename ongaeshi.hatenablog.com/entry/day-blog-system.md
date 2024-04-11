---
Title: 毎日簡単に更新できてサクサク閲覧できるブログシステムを作った
Date: 2022-07-19T23:43:34+09:00
URL: https://ongaeshi.hatenablog.com/entry/day-blog-system
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/4207112889900600722
---

Jekyll + GitHub Actions で構築。ブログの開設から記事の更新まで全てWebインターフェース上で完結します。

[https://github.com/ongaeshi/day:embed:cite]

[https://day.ongaeshi.me/] が実際に自分が使っているやつなので、これを見るとどんな感じのものが作れるか分かると思います。

[f:id:tuto0621:20220719234531p:plain]

## 記事の投稿
- マークダウンに日付を付けた見出し(`# 2022-07-19`)を作ると中身がその日の投稿になります(例: [_days/2022.md](https://github.com/ongaeshi/day/blob/main/_days/2022.md))
- `# 2022-07-19 今日の出来事`のように日付の後ろに空白を開けて記事にタイトルを付けることもできます
- `_days/`ディレクトリ以下の全ての`.md`ファイルが投稿対象になります(ファイルの分割単位は自由です)

記事の投稿に新規ファイルの追加が不要なため、編集用のURLをブックマークしておいて一番上に見出しを追加すれば記事の投稿が完了します。
1ファイルから複数の記事を発行できるので、コンテンツの移動や統合も簡単です。

## 新規ブログの開設の仕方
[https://github.com/ongaeshi/day#%E3%83%87%E3%83%97%E3%83%AD%E3%82%A4] の手順に沿って進めます。

GitHubにtemplateという仕組みを使っているため、**Use this template**ボタンをクリックして自分のレポジトリを作って、あとは自分の設定をしてください。

## カスタマイズ
中身は Jekyll なので好きにやってください。

うまく動かない、使ってみたなどありましたら[@ongaeshi](https://twitter.com/ongaeshi)まで教えていただけると喜びます。 
