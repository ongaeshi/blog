---
Title: Ruby On BrowserとRuby WASM/WASIの雑感
Date: 2022-03-26T23:17:55+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubyonbrowser-and-rubywasmwasi
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/13574176438076974626
---

Ruby WASM/WASI の発表にえらくテンションが上がったので、勢いで作ったものが[窓の杜で紹介](https://forest.watch.impress.co.jp/docs/serial/yajiuma/1397787.html)されてびっくりしました。(それだけ注目されているということですね)

[Ruby On Browser](https://ongaeshi.github.io/rubyonbrowser/)は51行しかないHTMLでまだまだ荒削りなのでもっとちゃんとしたものを試したい方は是非[TryRuby playground](https://try.ruby-lang.org/playground/)のCRuby 3.2.0dev をお試しください。

Ruby On Browser自体もまだまだ発展させていくつもりですが、現状Ruby WASM/WASIを触ってみていいなあと思ったことです。

## 1. 簡単に自分好みのブラウザRubyが作れる
Try Rubyのようにブラウザ上でプログラミング言語が試せること自体は現在はそこまで珍しくないですが、クライアントサイドだけで(しかもとても短いコードで)動かせるのは大変魅力的です。個人のPCやイントラネット上に好みのカスタマイズを加えたブラウザRubyを簡単に構築することができます。

## 2. wasmファイルの入れ替えが簡単
Webアプリ自体はそのままにバイナリファイルの ruby.wasm を入れ替えるだけで別バージョンのRubyを試すことができます。

## 3. 最新版がすでにNightlyビルドされている
すでにCRubyのレポジトリに取り込まれているため常に最新の.wasmが動くのも大きいです。[Nightlyビルド](https://github.com/kateinoigakukun/ruby.wasm/releases) を使えば一番簡単に最新版を試せる環境となりそうです。(2022-03-25現在のruby.wasmは9.8MiBなので1週間分保管しておいても70MiB程度)

[f:id:tuto0621:20220326231418p:plain]

## 4. ファイルAPIが使える
内部で使っている @wasmer/wasmfs はメモリ上に仮想ファイルシステムを構築できるため、File.read, File.write のようなファイルAPIもブラウザ上で動かすことができます。(今実験中です)

WASI自体はファイルシステム以外にソケット通信などもサポートしているため、将来的には `require "net/socket"`や`require "net/http"`もできるようになるかもしれません。夢が広がりますね。

