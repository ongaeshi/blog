---
Title: ブログ執筆を加速させるコマンドラインインターフェースを作成した
Category:
- blogsync
- ruby
Date: 2024-07-11T09:10:56+09:00
URL: https://ongaeshi.hatenablog.com/entry/2024/07/blog-writing-interface
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189120988641
---

このブログに素早く投稿するために、個人用のコマンドラインインターフェース(CLI)を作った。

[https://github.com/ongaeshi/blog_command:embed:cite]

- 内部で [blogsync](https://github.com/x-motemen/blogsync) を使っている。
- Ruby + Thor で作成し、[runa](https://ongaeshi.hatenablog.com/entry/2023/07/23/113420) で `blog` という名前のコマンドとして、どこからでも実行できるようにしてある。

# 投稿: blog new
新規記事を作成するときは new コマンドを使う。引数として URL を指定するカスタムパスを渡す。

```
$ blog new path/to/article
     store /Users/ongaeshi/blog/ongaeshi.hatenablog.com/entry/path/to/article.md
```

- 下書きとしてはてなブログ上に空の記事が投稿される。
- URL は `https://ongaeshi.hatenablog.com/entry/path/to/article` になる。
- 対応するファイルが `/Users/ongaeshi/blog/ongaeshi.hatenablog.com/entry/path/to/article.md` に作成される。

blog new 直後の *path/to/article.md*。

```md
---
Title:
URL: https://ongaeshi.hatenablog.com/entry/path/to/article
EditURL: ...
PreviewURL: ...
Draft: true
---

(ここに記事を書いていく)
```

# 執筆: blog edit 

```
$ blog edit
```

- ブログ記事が格納された git レポジトリをVSCode で開く。
- blog new で作成したファイルを開いて記事を書いていく。
- 記事の執筆が終わったら `draft: true` の行を削除して `blog commit` コマンドを実行する。

# 投稿: blog commit

```
$ blog commit

または

$ blog commit -m "/path/to/article を投稿"
```

- 裏で blogsync push してから git commit を行う
- blogsync push する記事は以下のどれか
  - まだ追加されていないファイル (`git status --porcelain`)
  - 編集済みのファイル (`git diff --name-only`)
  - main に pushしていないファイル (`git diff --name-only origin/main..HEAD`)
- blogsync push と git commit を同時に行うことでどちらかの実行し忘れを防ぐ。(blog コマンドを作る前はこの操作ミスが本当に多かった)

# 感想
ここ１か月ほど、快適にブログ執筆するためのインターフェースはどうなっているのがよいかを試行錯誤していた。
個人的には特に以下の辺りが重要だった。

1. ブログのカスタムパスは必ず指定する。
1. `blogsync post` では標準入力を受け付けずに自動で空の記事を作成する。
1. 空の記事がデフォルトになるので必ず下書きで作成する。(オプションからは指定できないようにする。)
1. git commit と blogsync push を1つのコマンドできるようにする 

自分用コマンドを作った効果は抜群で今月だけでだけですでに**7本**の記事を執筆できている。
今年6月までに書いた記事が**10本**なので1か月で半年分を追い抜きそうなペース。

よいインターフェースには利用者の力をブーストする、もしくは最大限に発揮させる力があると改め感じた。

# まとめ
blogsync に薄いラッパーを被せて自分のブログ投稿ワークフローに最適なインターフェースを作った。
ユーザーにフィットしたインターフェースをデザインすることは、個人や組織の開発効率を飛躍的に向上させる。

今や、重要な内部パーツは多くがOSSとして既に存在している、あるいはLLMによって生成できる時代になった。
そのため、様々な状況に対応できるインターフェースを作成することが、これからのプログラマにとって一層重要なスキルとなりそうな予感がする。
