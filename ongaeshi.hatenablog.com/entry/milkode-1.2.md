---
Title: ' Milkode 1.2 リリース :  ファイル名のマッチ個所に色づけ、ctags連携、~/.gitignoreのサポート'
Category:
- groonga
- ruby
- milkode
Date: 2013-08-10T22:29:55+09:00
URL: https://ongaeshi.hatenablog.com/entry/milkode-1.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318756616065
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130810/20130810193312.png" alt="f:id:tuto0621:20130810193312p:plain" title="f:id:tuto0621:20130810193312p:plain" class="hatena-fotolife" itemprop="image"></span></p>

- ファイル名のマッチ個所に色づけ 
- ctagsとの連携
- ~/.gitignoreのサポート
- milk update時にignore対象をデータベースから削除

## インストール
```
$ gem install milkode
```

[ダウンロード](http://milkode.ongaeshi.me/download.html), [Gems](https://rubygems.org/gems/milkode/versions/1.2.0)

## ファイル名のマッチ個所に色づけ
ファイル名のマッチ箇所がハイライトされて分かりやすくなりました。(↑画像)
他にも同じファイルで複数箇所にマッチした時はまとめたりとwebアプリの使い勝手を改善しました。

## ctagsとの連携
milk update と一緒にタグファイルを作成出来るようになりました。

```
# パッケージ単位で設定可能
$ cd /path/to/package

# milk update と一緒に'ctags -R'
$ milk config update_with_ctags true

# milk update と一緒に'ctags -R -e'
$ milk config update_with_ctags_e true
```

```
# 更新
$ milk update

# パッケージのルートディレクトリにTAGSファイルが作られる
$ ls
TAGS
.
.
```

milk config で設定可能な項目は`milk config -h `で確認出来ます。

```
$ milk config -h
Usage:
  milk config [options] KEY VALUE

Options:
  -d, [--delete]   # Delete key.
  -h, [--help]     # Help message.
      [--version]  # Show version.

Config package settings.

  $ milk coinfig no_auto_ignore true

Configs:
  no_auto_ignore           # Not add package's .gitignore
  update_with_ctags        # Update with 'ctags -R'
  update_with_ctags_e      # Update with 'ctags -R -e'
  update_with_git_pull     # Update with 'git pull'
  update_with_svn_update   # Update with 'svn update'
```

## ~/.gitignoreのサポート
全てのパッケージに適用されるグローバルな.gitignoreファイルを指定出来るようになりました。

```
$ milk ignore --global ~/.gitignore
```
## milk update時にignore対象をデータベースから削除
milk add した後に milk ignore で設定を追加して milk update してもいままで対象ファイルがデータベースから除外されなかったのですが、今回から除外されるようになりました。

何か設定を間違えていても**修正してmilk updateさえすれば大丈夫**になりました。

## リリースノート
* milk web
  * Hightlight filename keywords
  * Not show file infomation when same file name continue
  * Delete auto search feature 'w:0' and 'fp:a b'
  * Bugfix relative URL (clippy, 同名ファイルへのリンク) (thanks nicklegr)

* milk
  * milk update
    * Remove ignored files with update
  * milk ignore
    * Support global ignore. 'milk ignore --global ~/.gitignore'
  * milk config
    * Add 'milk config KEY VALUE' command.
    * e.g. 'milk config update_with_ctags_e true'
  * milk remove
    * Faster DocumentTable#remove_records
    * Not remove package non exist filename
  * milk cleanup
    * Add milk cleanup options. -p, -a
  * milk add
    * 'milk add' can't use already exist package

## 関連記事
- [Milkode 1.1 リリース : 待望の相対URLに対応、gmilkの高速化 ](http://ongaeshi.hatenablog.com/entry/milkode-1.1)
- [Rubyで特定のコマンドが存在するかを調べる方法](http://ongaeshi.hatenablog.com/entry/2013/06/18/144256)
- [Milkodeをgrepと組み合わせたら検索速度が40倍速くなった](http://ongaeshi.hatenablog.com/entry/milkode-and-grep)
- [行指向のソースコード検索エンジンMilkode1.0.0リリース!](http://ongaeshi.hatenablog.com/entry/milkode-1.0.0)
- [gihyo.jpに記事を書いた時の感想](http://ongaeshi.hatenablog.com/entry/write-article-on-gihyojp)



