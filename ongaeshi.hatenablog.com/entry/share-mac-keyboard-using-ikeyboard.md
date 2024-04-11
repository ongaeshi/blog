---
Title: MacのキーボードをiPhoneやiPadと共有して文字入力するならiKeyboardがベスト(JIS配列なら特に)
Category:
- ios
- osx
- mac
- iKeyboard
- diary
Date: 2015-04-23T10:25:49+09:00
URL: https://ongaeshi.hatenablog.com/entry/share-mac-keyboard-using-ikeyboard
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450092321059
---

iPhoneやiPadでもキーボードを使うと長い文章やプログラムを書く時に操作性が格段によくなるので、別のキーボードの購入や、OSXで使っているキーボードを一旦接続解除して再接続、など色々と試してみたのだけど結果としてはMacとのキーボード共有ソフトを使うのがよいという結論になりました。

- [Mac App Store - iKeyboard](https://itunes.apple.com/jp/app/ikeyboard/id441439411?mt=12)

仕組みとしてはiPhoneとキーボードを接続せずiPhoneとMacをBluetoothで接続しMacに入れた専用アプリ経由でiPhoneに文字を送信します。なかなかトリッキーなことをしているけど入力レスポンスの遅延なども特に感じませんでした。

[f:id:tuto0621:20150423012730p:plain]

iKeyboardはJISキーボードなどのUS以外の配列でもちゃんと動くのでおすすめです。1Keyboardなどの競合製品も試してみたのですが接続はうまくいったものの記号入力がUSになったり「かな」「英数」キーを認識してくれなませんでした(クリップボードの転送機能もあるのはよかったのですが)。そっちはPastebotでまかなっています。

Command+tabでiKeyboardに切り替えた時だけiPhoneに対して入力出来るようになるので使い勝手としては他のアプリ切り替えと同じ感覚で使えます。Twitterアプリなどを立ち上げておくと小さなセカンドスクリーン代わりにもなってよいです。スタンドはダイソーで100円でした。

[f:id:tuto0621:20150423012744j:plain]

[ofruby](http://ofruby.tokyo)のようなiOS環境でプログラムをもっと書きやすくするための仕組みというのを長いこと考えているのですが、キーボード対応をもっと手厚くすると大分捗る気がしたのでそっちを頑張っていうこうという気になってきました。

※ リンク先のコメント読むとローマ字入力出来ない等の情報もありますが私の環境(Mavericks)だと日本語入力も問題無く出来ています。怪しい時はPC側のIMEがONになっていないかチェックするとよいです｡

