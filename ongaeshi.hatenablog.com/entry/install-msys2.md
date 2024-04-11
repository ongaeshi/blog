---
Title: msys2をインストールする
Category:
- 開発環境
Date: 2018-01-21T21:09:51+09:00
URL: https://ongaeshi.hatenablog.com/entry/install-msys2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812339700166
---

新しいWindowsマシンの開発環境を整えるためにmsysをインストールした。WSLだけでなんとかなるかと思ったけど、コマンドラインツールをエディタから起動することができなかったり、特にgitがないのでmagitが起動できないのもあって不便になってきた。Web開発などWSLの方が便利なケースも多々あるため、しばらくは併用で使ってみようと思う。

- [Windows10の開発環境をMSYS2で再構築 - Qiita](https://qiita.com/azk0305/items/a546da060f3ab8d6a8bf)

↑に沿ってインストールしてみたけど、pacmanがうまく動かなくなってしまった。最新版で状況が変わっているのかもしれない。

- [MSYS2 homepage](http://www.msys2.org/)

次は公式に沿ってやってみる。

```
$ pacman -Syu
$ pacman -Su
```

うまくいった。WindowsのPATHを引き継ぎたいので環境変数`MSYS2_PATH_TYPE`に`inherit`を設定してからシェルを再起動する。

ホームディレクトリがバラバラなので、設定ファイルはそれぞれ独立している。

| 種類      | 場所                                |
| ------- | --------------------------------- |
| Windows | c:\Users\ongaeshi\AppData\Roaming |
| msys2   | C:\msys64\home\ongaeshi           |
| WSL     | Windowsから直接触ってはいけない               |

そのうち[ホームディレクトリを一か所にまとめ](http://www.clear-code.com/blog/2017/11/8.html)た方がよさそうだが、とりあえずはmsy2のホームにもコピーしてしのぐ。

```
$ cp /c/Users/ongaeshi/AppData/Roaming/.gitconfig ~/
```

これで、msys2が最低限動くようになった。

