---
Title: 今週のJUNKとANNをRadikoで一瞬で開くRubyスクリプト
Category:
- rubypico
Date: 2017-07-31T21:55:52+09:00
URL: https://ongaeshi.hatenablog.com/entry/radiko-junk-ann
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812284292951
---

Air Podsを買ってからポッドキャストやネットラジオを大分聞くようになった(片耳だけ付けるとモノラルになるのが素晴らしい)。

Radikoも使っているのだが最近になって有料の[エリアフリー](http://radiko.jp/rg/premium/)に入った。これでJUNK(TBSラジオの深夜放送)やANN(オールナイトニッポン)をタイムシフトかつ細切れに聞くことができる。

※ Radikoの放送は1週間いつでも再生できるのだが「(謎仕様で)再生開始から3時間」しか聴くことができない。しかし複数の放送局に配信している番組であれば放送局をまたがることで細切れにタイムシフト再生が可能になる。例えばJUNKのような2時間の番組なら4局をまたがることで30分細切れに再生が可能となる(そのためにもエリアフリーが便利)。

しかしスマホアプリのRadikoはこのような使い方をあまりサポートしてくれない。例えば「バナナ」で検索したら再生可能な「JUNK バナナマンのバナナムーンGOLD」を局毎に一覧表示してくれればいいのだが、なぜかTOPに表示されるのは来週放送予定の番組一覧だったりする。JUNKやANNだと少なくとも5局以上配信されているため、実際に再生できる番組は下までスクロールしないと見つからなくて辛い。さらに放送中に突然アプリが落ちたりする。さらにさらに履歴機能が無いため番組を聴いている最中にアプリが突然落ちてしまうと 再度番組名で検索→TOPに表示されるのは放送予定なので下までスクロール→辛い という手順を何度も繰り返す必要がある。

解決するためにプログラムを書くことにした。スマホから使いたいので[RubyPico](http://rubypico.ongaeshi.me/)で書く。

## junk.rb, ann.rb
プログラムはGitHubに置いた。RubyPicoGemsの`github_download.rb`を使っている人は`radiko`でダウンロードできる。

[https://github.com/rubypico/radiko:embed:cite]

junk.rbが[TBS JUNK](https://www.tbsradio.jp/tag/%EF%BD%8A%EF%BD%95%EF%BD%8E%EF%BD%8B/)用、ann.rbが[オールナイトニッポン](http://www.allnightnippon.com/)用。

実行すると以下のような表が出力される。リンクをタップするとRadikoが立ち上がり番組を
任意の局で再生できる。何時クリックしても一番最近に放送された番組が立ち上がるのが便利なところ。

[f:id:tuto0621:20170731212520j:plain]

## 解説
Radikoにはシェア機能というよい機能があり<b>URLで任意の番組を好きな時間から再生</b>することができる。

http://radiko.jp/share/?sid=LFR&t=20170725010000

例えば上のURLは`sid=LFR`(放送局はニッポン放送)、`t=20170725010000`(2017/07/25 01:00から再生)となる。番組の最初からではなく途中からでも再生できるのがよい。

今回は途中再生は不要、かつすべて1:00から放送なのでタップした時間から一番最近の月～金の1:00を求めて表を作ればよい。

## 最近の曜日を求める
RubyPico(mruby)にはDateTimeが無いのでそこはTimeだけで頑張る必要があった。

[radiko/radiko.rb at master · rubypico/radiko](https://github.com/rubypico/radiko/blob/master/radiko.rb)

```ruby
def last_wday(wday, hour, min)
  t = Time.now
  t = Time.local(t.year, t.month, t.day, hour, min)
  if t.wday >= wday
    d = t.wday - wday
  else
    d = t.wday + 7 - wday
  end
  t - d * (24 * 60 * 60)
end
```

## まとめ
スクリプトのおかげでJUNKとANNの再生がとても捗るようになった。また細切れに聞くのも簡単になったので通勤時間を利用して2～3回に分けて1つの番組を聴くのが基本スタイルになった(本当はどこまで聴いたかを記録できるようになるといいのだが)。

困ったこととしてはRubyPicoからの起動に慣れすぎて他の番組をなんなく聴かなくなってしまったことだ。URLを叩くためのライブラリ層は大体構築できたので次は自分のお気に入り番組を素早く再生するスクリプトを作りたいと思う。

