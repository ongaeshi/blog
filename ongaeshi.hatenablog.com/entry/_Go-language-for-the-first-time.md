---
Title: はじめてのGo言語
Category:
- go
Date: 2014-04-21T00:24:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/_Go-language-for-the-first-time
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815722364788
---

<b>pt</b>([monochromegane/the_platinum_searcher](https://github.com/monochromegane/the_platinum_searcher))を改造してみたくなり(つまりはWindowsでも動くagが欲しくて)、久しぶりに新しい言語が覚えたくなったのでGo言語をはじめてみました。

以下、最初にひっかかった所を含めてメモ。

## インストール

```
$ sudo port install go (or brew install go)
```

続いてGOPATHの設定。ソースコードやライブラリが置かれる場所だと理解している。

```
$ export GOPATH=$HOME/Documents/gocode
```

定まったら`.bashrc`に追記しておく。

### Mercurial のインストール
`go get` (ライブラリをインストールするコマンド) は Git や Svn や Mercurial 等をライブラリ提供者側が選択出来るらしい。で、Google製のライブラリは Mercurial で提供されているようなので Git しか入っていない人はここで Mercurial をインストールしておく。

```
$ sudo port install mercurial
```

### go get でトラブル
一番引っかかった所。

```
$ go get github.com/PuerkitoBio/goquery
```

に失敗する。エラーが起きない人は次に進んでよい。
curl-ca-bundle を再インストールすることで動くようになった。

```
$ sudo port install curl-ca-bundle
$ sudo port -f activate curl-ca-bundle @7.36.0_0
--->  Activating curl-ca-bundle @7.36.0_0
Warning: File /opt/local/etc/openssl/cert.pem already exists.  Moving to: /opt/local/etc/openssl/cert.pem.mp_1397399953.
--->  Cleaning curl-ca-bundle
```

### Emacs用のgo-modeをインストール
とりあえずどんな言語でもメジャーモードが用意されているのは Emacs のよい所。

```
M-x package-install go-mode
```

Goは基本インデントがタブなので、タブ幅を4に設定しておく。

```
(setq default-tab-width 4)
```

Emacs は go-mode が良く出来ていて特に気にせずタブに変換してくれるのでそんなに困らないけど、普段タブを使わないエディタ設定を使っている人は注意。(空白でもとりあえず書けるようではある。)

## チュートリアルをやってみる
[A Tour of Go](http://tour.golang.org/) ([日本語版](http://go-tour-jp.appspot.com/#1))

を少しづつ進めるとよい。

例えば[#3 The Go Playground](http://tour.golang.org/#3)を`3.go`に保存したら、

```go
package main

import (
    "fmt"
    "net"
    "os"
    "time"
)

func main() {
    fmt.Println("Welcome to the playground!")

    fmt.Println("The time is", time.Now())

    fmt.Println("And if you try to open a file:")
    fmt.Println(os.Open("filename"))

    fmt.Println("Or access the network:")
    fmt.Println(net.Dial("tcp", "google.com"))
}
```

`go run`で実行出来る。

```
$ go run 3.go
Welcome to the playground!
The time is 2014-04-20 15:49:47.334389539 +0900 JST
And if you try to open a file:
<nil> open filename: no such file or directory
Or access the network:
<nil> dial tcp: missing port in address google.com
```

`go build`でバイナリを作成。

```
$ go build 3.go
$ ./3            # 実行
Welcome to the playground!
The time is 2014-04-20 15:51:22.174033453 +0900 JST
And if you try to open a file:
<nil> open filename: no such file or directory
Or access the network:
<nil> dial tcp: missing port in address google.com
```

普段	の開発中はほとんど`go run`しか使わなくてよい、スクリプトっぽく動かせて便利。

## the_platinum_searcher をビルドする
[monochromegane/the_platinum_searcher](https://github.com/monochromegane/the_platinum_searcher) の通りなのだけど、`go get`して、`cd`して、`go build`する。

```
$ go get github.com/monochromegane/the_platinum_searcher
$ cd $GOPATH/src/github.com/monochromegane/the_platinum_searcher
$ go build -o ~/tmp/pt
```

最後だけ`~/tmp/pt`に変えてるけど要はバイナリの出力先なので好きな所に。

ここから<b>pt</b>を改造していくのだけどそれはまた次の機会に。

## Go言語初感
触って一週間位の人の感想です。

- Windows, OSX, Linux で動くバイナリを簡単に作れるのはかなりよい
- CやC++に比べると記述量を減らそうとかなり努力している
  - C/C++ > D, C#, Go > Ruby, Python の印象
  - なによりヘッダを書かずにネイティブコードが吐けるのは嬉しい (Dもだけど)
- 静的型なので型チェックによってテストを書かなくてもある程度の安全性が保証される
- `go run`を使えば実行まで1ステップなので開発中のターンアラウンドはスクリプト言語のそれに近い
  - もっと規模の大きなコードを書くと印象が変わるかもしれないけど
- 本当のウリはgoルーチンを中心とする並列化処理だったりするのだろうけどその辺はまだ触ってないです、すいません
- `:=`の初期値代入が好き

まずは、他言語のライブラリに余り依存していなくて(もしくはGo言語向けのライブラリがすでに存在していて)、最終的にバイナリ形式で配布したいものに使ってみようかと思います。
