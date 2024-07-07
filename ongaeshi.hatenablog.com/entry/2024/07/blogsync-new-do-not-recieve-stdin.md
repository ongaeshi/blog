---
Title: blogsync new は標準入力を受け取らない＆コミットしない
URL: https://ongaeshi.hatenablog.com/entry/2024/07/blogsync-new-do-not-recieve-stdin
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189120259970
PreviewURL: https://ongaeshi.hatenablog.com/draft/entry/GnE5z5aDhG10fYl5de-4Cx2ilvI
---
[前回](https://ongaeshi.hatenablog.com/entry/2024/07/implement-blog-new-command) から引き続き blog new コマンドの改善を続けている。

直接記事をポストしたいときは標準入力から記事の中身を受け取れるのは便利だけど、
自分の場合は VSCode ですぐに編集するのでまず空の標準入力を渡してその処理をキャンセルする。

```ruby
      system("blogsync post --custom-path #{path} #{opts.join(" ")} ongaeshi.hatenablog.com", in: IO::NULL)
```

そうすると記事の最初の一回目は必ず空の状態で post されることになるので、常に`--draft`として作成することに変更。

```ruby
      system("blogsync post --custom-path #{path} --draft #{opts.join(" ")} ongaeshi.hatenablog.com", in: IO::NULL)
```

blog new コマンドでは空記事の作成のみにとどめてブログへの投稿は blog commit コマンドで常に行うになった。
大分驚きが減ってすっきりしたオペレーションになったのではないか。

最終的な blog new コマンド。

```ruby
```
