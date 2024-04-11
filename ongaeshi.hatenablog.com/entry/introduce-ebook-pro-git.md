---
Title: PCから読めるようにしておくと超絶便利な電子書籍- Pro Git 日本語版
Category:
- book
- honyomi
Date: 2015-07-08T01:06:12+09:00
URL: https://ongaeshi.hatenablog.com/entry/introduce-ebook-pro-git
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450100659969
---

[Honyomi 1.2](http://ongaeshi.hatenablog.com/entry/honyomi-1.2)でPCから使える理想の電子書籍リーダーに(個人的に)大分近づいたので、布教も兼ねて自分用のHonyomiに追加してよく使っている電子書籍を紹介していこうと思います。

本を買って最初の1回目は紙の本で読んだりKindleで読んだほうが読みやすいのですが、技術書は実際の問題に遭遇したときにさっと検索して目的のページを読み返すことで効率的に学習することができるので、PCから読めるようにしておくとおすすめです。

## Pro Git 日本語版

[Pro Git 日本語版電子書籍公開サイト](https://progit-ja.github.io/)

このクオリティの書籍が無料で配布されているのに驚きです。Gitを使っているのであれば一度は目を通しておくことをおすすめします。pdf以外にもepub, mobi と一通りの形式が揃っているようです。

CC BY-NC-SA 3.0 で配布されているので本読みの図書館にも置かせてもらっています。

http://honyomi.ongaeshi.me/v/4

せっかくなので、本の中で面白かったトピックをいくつか紹介します。

## 3.5.3 リモートブランチの削除

[3.5.3 リモートブランチの削除](http://honyomi.ongaeshi.me/v/4?query=%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E3%81%AE%E5%89%8A%E9%99%A4&page=87)

> $ git push origin :serverfix

実行するたびに忘れるやつです。本書ではコマンドを覚えるためのコツまで教えてくれます。

> このコマンドを覚えるコツは、少し前に説明した構文 git push [remotename] [localbranch]:[remotebranch]を思い出すこ
とです。[localbranch]の部分をそのまま残して考えると、これは基本的に「こっちの (何もなし) で、向こうの[remotebranch]を更新しろ」と言っていることになります。

## 6.5.2 二分探索

[6.5.2 二分探索](http://honyomi.ongaeshi.me/v/4?page=193&query=193)

こちらも数ヶ月に1回位しか使わないので使うたびに読み返しています。`git bisect`を始めて使ったときは頭のいい人がいるものだなぁと素直に感心したのを覚えています。

## 	1.3 Gitの基本

[1.3 Gitの基本](http://honyomi.ongaeshi.me/v/4?query=git%E3%81%AE%E5%9F%BA%E6%9C%AC&page=14)

「スナップショットで、差分ではない」「ほとんど全ての操作がローカル」など他のバージョン管理システムと何が違うのか(革命的だったのか)をとても分かりやすく説明してくれます。

## 9章 Gitの裏側

[9章 Gitの裏側](http://honyomi.ongaeshi.me/v/4?page=265&query=265)

Gitの裏側で動いているメカニズムを紹介してくれます。Gitはもともとは連想記憶ファイルシステムなのです。(だから低レイヤーを操作するコマンドがたくさん用意されているのですね。)

## 3.6.3 ほんとうは怖いリベース

[3.6.3 ほんとうは怖いリベース](http://honyomi.ongaeshi.me/v/4?query=%E3%81%BB%E3%82%93%E3%81%A8%E3%81%86%E3%81%AF%E6%80%96%E3%81%84%E3%83%AA%E3%83%99%E3%83%BC%E3%82%B9&page=91)

> 公開リポジトリにプッシュしたコミットをリベースしてはいけない
>
> この指針に従っている限り、すべてはうまく進みます。もしこれを守らなければ、あな たは嫌われ者となり、友⼈や家族からも軽蔑されることになるでしょう。

😫😫😫😫😫😫😫😫😫

## 目次

[目次](http://honyomi.ongaeshi.me/v/4?page=3&query=3)

キーワードが思いつくときは検索を使いますが、そうじゃないときはとりあえずざっと目を通します。

## まとめ
よい本を[いつでも検索、閲覧出来るように](http://ongaeshi.hatenablog.com/entry/honyomi-web)しておくと色々捗るのでおすすめです。


