---
Title: gitで複数のリモートレポジトリにpushする方法
Category:
- git
- GitHub
Date: 2013-10-15T22:50:03+09:00
URL: https://ongaeshi.hatenablog.com/entry/how-to-push-to-remote-repository-of-multiple-git
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318758789527
---

git使ってて都合上push先が2つになることがあって、毎回

```
$ git push              # originにpush
$ git push origin2   # それ以外にpushする必要のあるレポジトリ
```

・・が面倒なので何とかしたかった、探したら[あった](http://d.hatena.ne.jp/Naruhodius/20110418/1303111779)。

`.git/config`に

```
[remote "origin"]
	url = git@github.com:ongaeshi/milkode.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

一行追加すればOK。

```
[remote "origin"]
	url = git@github.com:ongaeshi/milkode.git
+	url = git@github.com:hogehoge/hogehoge.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

`git push`の度に2つのレポジトリにpushしてくれる。pullの時はどうなるのだろう？

```
$ git push
```

しばらく使ってみる。

## 参考文献
- [git で複数のリモートリポジトリに一度に push したりする。 - D.](http://d.hatena.ne.jp/Naruhodius/20110418/1303111779)
