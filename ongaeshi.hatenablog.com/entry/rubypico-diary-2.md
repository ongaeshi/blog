---
Title: RubyPico開発日記2 - SimpleHttpServerを2回起動するとserver.rb:50 bind (RuntimeError)
Category:
- rubypico
Date: 2016-07-31T15:41:10+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-diary-2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687176861206
---

こんな感じ。

[f:id:tuto0621:20160731154021p:plain]

おそらく一度ソケットで確保したアドレスがプログラム終了後にcloseされずに確保しっぱなしになっているようだ。RubyPico自体を終了させると解放される。

CRubyはプロセスとして起動する場合が多いのでWebサーバーを止めたいときはそのプロセスを停止してしまえばよい、OSが安全に後始末してくれる。ところがiOSのアプリは子プロセスが作れない(forkできない)ので、RubyPicoは別スレッドでmrubyを動かしているのでプログラム終了時にこちらでなんとかしないといけない。プログラム終了後にはmrb_close()が呼ばれるのでそのタイミングで未解放のソケットがあればcloseする、例えばGC時に後始末関数を呼ぶような仕組みがいいのだろうか。

Rubyには[Finalizer](http://komamitsu.hatenablog.com/entry/20090426/1240751698)があったがgrepした感じmrubyにはない。なのでRubyPicoの[Imageクラス](https://github.com/ongaeshi/RubyPico/blob/master/App/mrb_image.m#L22)のようにcloseされていなかったら解放時にcloseを呼ぶようなライブラリを用意するのがいいのかしら？

もしくはiOS用に用意された[CocoaHTTPServer](https://github.com/robbiehanson/CocoaHTTPServer)というのがあるのでこいつをRubyPico用にバインドするのもいいかもしれない。でも、低レイヤのソケットライブラリが用意されているとどうやってHTTPが動いているのかを勉強するのにいいんだよなぁ、実際mruby-simplehttpserverはたった180行のRubyスクリプトで[書かれて](https://github.com/matsumoto-r/mruby-simplehttpserver/blob/master/mrblib/mrb_simplehttpserver.rb)いて、私自身Webサーバがどうやって動いているのか理解するのにとても役立った。runとか関数の内容をRubyPico上でオーバーライドすれば簡単に挙動を変えて確かめられるしね。(ここがRubyのすごいところだと思う)
