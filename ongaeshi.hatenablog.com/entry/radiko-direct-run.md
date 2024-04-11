---
Title: Radikoの番組をRubyPicoから直接開けるようになった
Category:
- rubypico
Date: 2017-08-09T18:30:52+09:00
URL: https://ongaeshi.hatenablog.com/entry/radiko-direct-run
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812287119506
---

[その後](http://ongaeshi.hatenablog.com/entry/radiko-junk-ann)もradiko/radiko.rbの改造を加えている。

## 任意の番組リストを作れるように
前回の課題になっていたやつ。自分の好きな番組もann.rbやjunk.rbのように表示できるようにした。

[rubypico/radiko - GitHub](https://github.com/rubypico/radiko)

こんな風に書くと

```ruby
# coding: utf-8
require "radiko/radiko"
include Radiko

c "山下達郎のサンデー・ソングブック", "Sun 14:00", %w(FMT RADIONERRY FMO E-RADIO FMAICHI FMGIFU)
c "ライムスター宇多丸のウィークエンドシャッフル", "Sat 22:00", %w(TBS RAB YBC RBC)
c "菊地成孔の粋な夜電波", "Sun 20:00", %w(TBS IBC YBC RFC BSN)
```

こんな風に表示してくれる。

[f:id:tuto0621:20170809182851p:image]

メソッド`Radiko::c`は今のところ週間番組を想定しているけど、以下のように書けば月～金だけ放送するような番組でもいける。

```ruby
# coding: utf-8
require "radiko/radiko"
include Radiko

CHANNELS = %w(TBS RAB CBC)

c "毎日ニュース(月)", "Mon 14:00", CHANNELS
c "毎日ニュース(火)", "Tue 14:00", CHANNELS
c "毎日ニュース(水)", "Wed 14:00", CHANNELS
c "毎日ニュース(木)", "Thu 14:00", CHANNELS
c "毎日ニュース(金)", "Fri 14:00", CHANNELS
```

## Radikoアプリを直接開けるようになった

めちゃ便利になった。


[https://twitter.com/ongaeshi/status/895160964689145856:embed#RubyPicoのradiko.rbアプリ、今までhttps://t.co/wOX27Xv55x.. をブラウザで開きそこからアプリ起動していたのだが、 URLを radiko://radiko.jp/.. にすれば直接開けるのが分かって感動している]

[https://twitter.com/ongaeshi/status/895207452496412672:embed#これを変えるだけでブラウザ経由せずに直接アプリ起動できるらしい - Change URL from "http" to "radiko" · rubypico/radiko@3442542 https://t.co/5wkfU6rH5V]

[https://twitter.com/ongaeshi/status/895209710890045440:embed#RubyPicoからRadikoダイレクト起動の様子 https://t.co/KArKICnM1a]


