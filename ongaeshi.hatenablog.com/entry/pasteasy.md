---
Title: クリップボードを安全に共有できるPasteasyが便利(iOSからPCへのコピーはウィジェット経由で)
Category:
- ios
- mac
- rubypico
Date: 2015-12-16T23:29:48+09:00
URL: https://ongaeshi.hatenablog.com/entry/pasteasy
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347148814842
---

(2019/05/02 追記) コメントで教えていただきました。Pasteasyは既にダウンロードできなくなっているようです。

iOS9になってクリップボード共有ツールのpastebotが使えなくなってしまいました。[PictRuby](http://pictruby.ongaeshi.me/)を使っているとモバイルで書いたコードをPCにコピーしてGitHubにコミットしたり、PCでコードを軽くリファクタリングしてからモバイルに戻したりする作業が頻発したのでますます困るように。(Dropbox上に共有テキストファイル作ってみたりGistを使ったりしたけどやっぱり面倒臭い・・)

色々と物色したのですが[Pasteasy](http://www.pasteasy.com/)が良かったので紹介します。

- 無料(なぜ？)
- サーバー経由しないので安全
- 画像も送れる

## インストール
[https://itunes.apple.com/jp/app/pasteasy-iphonekarakopi-konpyutanipesuto/id883950743?mt=8:embed]

- iOS, PC(or Mac)にそれぞれpasteasyをインストール
- QRコードが表示されるのでPCで表示してiOSから撮影すると同期完了(逆でも良い)

同期の仕方はpastebotよりも遥かに簡単になっていて時代の流れを感じます。

## 使い方
### PCからiOSにテキストや画像を送信
この辺りはpastebotと同じです。

- 同期済みのpasteasyをそれぞれのマシンで起動
- PCでコピーすると自動でiOSに送られる

### iOSからPCにテキストや画像を送信
ウィジェットを経由する必要があります。

- 通知センターの「Pasteasy」をインストール
- お互いのpasteasyを起動
- 対象をコピーしたら通知センターを上からさっと開いて
- 送るボタンを押すとPCに送信されます

Q&Aを見るとiOS9の制約らしいですね。

> Why am I unable to send text/photos from my iOS device?  
> If you are using iOS9, please use one of the options below:  
> ✓Select a text or photo, press "share", then "send via Pasteasy".  
> ✓Alternatively, install the Pasteasy widget in the notification sheet. After you copy a text or photo, access the Pasteasy widget on the notification sheet and press send.</ol>  
> This change was introduced in response to changes Apple made in iOS9.  

### バックグラウンド3分の制約を突破する
iOSはアプリケーションがバックグラウンド状態で待機状態になると3分後にスリープ状態に入ってしまいます。これがアプリを起動してからしばらく経つと再度iOSアプリを起動するまでコピーできなくなる理由なのですが・・

Pasteasyはその間もBluethoothでつなげることで3分以上維持することができるらしいです

> Why does Pasteasy go to sleep in 3 minutes on my iPhone/iPad?  
> Unfortunately iOS restricts the Pasteasy app to go to sleep after 3 minutes of being in the background. To continue using Pasteasy, simply re-launch the app. If you have connected the iOS app to a Mac or another iOS device, please enable Bluetooth on the devices to prevent the iOS app from going to sleep. There is insignificant impact on battery life as Pasteasy uses Bluetooth LE. Sometimes the iOS app running in background might be terminated by the system. In this case please re-launch the app.  
> Pasteasy goes to sleep on my iPhone in 3 minutes even though I have bluetooth enabled on both my Mac and iPhone.  
> Just disable and enable Bluetooth on your iPhone. Watch the Bluetooth icon in the status bar &#45; it should flicker and turn solid white. If not, repeat till it does.

残念ながら私のiMacは古くてBluethoothLEが使えないせいかうまく動きませんでした。

> How do I know if my Mac has Bluetooth LE hardware?  
> The following Mac models have Bluetooth LE:  
> ✓MacBook Air (Mid 2012 and later)  
> ✓MacBook Pro (Mid 2012 and later)  
> ✓iMac (Late 2012 and later)  
> ✓Mac mini (Late 2012 and later)  
> ✓Mac Pro (Late 2013)</ol>  

## まとめ
面倒ですが、よくある質問を翻訳アプリを使って読みましょう。割と書いてあります。
