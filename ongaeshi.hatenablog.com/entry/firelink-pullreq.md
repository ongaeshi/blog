---
Title: FireLinkがプルリクエストだけでバージョンアップしました
Category:
- firefox
- firelink
- javascript
- web
Date: 2013-10-11T10:13:41+09:00
URL: https://ongaeshi.hatenablog.com/entry/firelink-pullreq
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318758633315
---

Firefoxのリンク生成用プラグイン、FireLinkが2.3.0にアップデートしました。

[FireLink - Copy link with keyboard shortcuts](https://addons.mozilla.org/en-US/firefox/addon/firelink/)

## 更新履歴

- [#3](https://github.com/ongaeshi/firelink/pull/3) オプションで自分の好きな短縮URLサービスを設定出来るように (thanks chihchun)
- [#2](https://github.com/ongaeshi/firelink/pull/2) canonical/shortlink/shorturl 属性をサポート (thanks chihchun)
- Firefox23で起きていたリンク編集画面(Ctrl+Shift+C)でセレクトボックスを開くと画面がおかしくなるバグがFirefox24で修正 (thanks firefox)
- **AUTHORSファイルを追加**

・・という訳で今回のアップデートはプルリクエストが中心で私がやったのはAUTHORSファイルを追加したのみ、という充実のアップデートになっております。

よいプルリクエストがもらえた時はソフトウェアを公開していてよかったなー、としみじみ感じます。

## 好きな短縮URLサービスを設定出来るように
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20131011/20131011005029.png" alt="f:id:tuto0621:20131011005029p:plain" title="f:id:tuto0621:20131011005029p:plain" class="hatena-fotolife" itemprop="image"></span></p>


設定項目(コンテキストメニューから [FireLink] -> [設定])に *Shortening service:* の欄が増えました。

デフォルトではis.gdを使っていますが、URLを設定する事で好きな短縮サービス(bitl.yやtinyurlなど)を使って短縮する事が出来ます。

### 設定例

- tinyurl
  - http://tinyurl.com/api-create.php?url=%URL%
- bit.ly
  - http://api.bit.ly/v3/shorten?format=txt&login=username&apiKey=XXX&longUrl&longUrl=%URL%

短縮URLは今まで通り *%isgd%* か、 *%short%* で生成することが出来ます。

今まで入った機能など、詳しい使い方については[コチラ](https://addons.mozilla.org/en-US/firefox/addon/firelink/)をどうぞ。


