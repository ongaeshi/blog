---
Title: Ruby on Browser 1.0 リリース
Category:
- ruby
Date: 2022-04-17T00:15:46+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubyonbrowser-1.0.0
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/13574176438083575215
---

[窓の杜](https://ongaeshi.hatenablog.com/entry/rubyonbrowser-and-rubywasmwasi)で取り上げていただいた後も実装は少しずつ続けていて、ひとまずブラウザ上で最新のRubyを試すのに必要な機能は一通り実装できたんじゃないかと思う。リファレンスマニュアルへのリンクを貼ってシンタックスハイライトを入れたりCtrl+Enterで実行できるようにした。(自分が書いたサンプルコードは[Scrapbox](https://scrapbox.io/rubyonbrowser)にあるのでコピペして試せます)

https://rubyonbrowser.ongaeshi.me/

[f:id:tuto0621:20220417001111p:plain]

モバイルでも簡単なコードだったら書けるように色々工夫したのでちょっとしたコードを書きたいときにぜひ試してみてほしい。(Select Allボタンは結構こだわった)

[f:id:tuto0621:20220417001229p:plain]

他のブラウザ言語処理系と大きく違うこととして「ファイルを読み書きするAPIも使える」ということがある。元々WASIがWASMにファイルIOや通信を持せたることを目的にしたものなのでRuby WASM/WASI自体がファイルIOを持っており、そこにWasmerFSというブラウザ上に仮想ファイルシステムを持たせることができるライブラリと組み合わせて実現している。

おかげで以下のようなサンプルコードをブラウザ上で実行できる！PC上で実行するよりもはるかに安全に色々試せるのはとてもよい。(ファイルはブラウザリロードで消える)

https://docs.ruby-lang.org/ja/latest/class/File.html#I_TRUNCATE

[f:id:tuto0621:20220417001133g:plain]

まだまだ伸びしろを感じているのでもうしばらくRuby WASIで遊ぶ予定。

