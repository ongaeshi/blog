---
Title: はてなブログを git レポジトリで運用する
Category:
- blogsync
- programming
Date: 2024-04-17T22:30:24+09:00
URL: https://ongaeshi.hatenablog.com/entry/hatena-blog-git-repository-management
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189097903098
---

過去のすべてのブログ記事を [git レポジトリで管理](https://github.com/ongaeshi/blog)することにした。

記事の管理や投稿には blogsync という Go 製のツールを使っている。

[https://github.com/x-motemen/blogsync:embed:cite]

blogsync をインストールしたら、以下のコマンドを叩くだけで記事の取得や投稿、更新ができる。

```
# レポジトリに移動
$ cd ~/Document/blog/

# 更新
$ blogsync pull ongaeshi.hatenablog.com

# 投稿＆更新
$ blogsync push ongaeshi.hatenablog.com\entry\2023\04\16\152439.md
```

原稿がローカルにあるので、一度投稿した記事を微調整したり、検索して過去記事にリンクを貼ったり、[複数記事のカテゴリをまとめて編集する](https://github.com/ongaeshi/blog/commit/3d8eb30529754391b4b431dca719a95bfabfe6b4)などがとてもやりやすくなった。

コマンドラインで記事の取得や投稿ができるので他のコマンドとの連携もやりやすい。例えば現在編集中の記事だけを再投稿したい場合は以下のようなコマンドを叩けばOK。

```
$ git diff --name-only | ForEach-Object { blogsync push $_ }
```

お気に入りのエディタを使ってドキュメントを書くのが好きな人など、 blogsync + git レポジトリの組み合わせによるブログ編集は大変おすすめです。

[blog:g:11696248318754550880:banner]
