---
Title: RubyPico 0.9.7 をリリース - mruby 1.3 に対応
Category:
- rubypico
Date: 2018-01-09T00:27:58+09:00
URL: https://ongaeshi.hatenablog.com/entry/mruby-0.9.7
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812335185196
---

mruby 1.3 に対応したり、`Browser.open`を連続でできるようにしました - [更新履歴](https://github.com/ongaeshi/RubyPico/blob/master/HISTORY.md#097---2018-01-06)

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## mruby 1.3 に対応
- Safe navigation operator (&.)
- Array#dig, Hash#dig
- Object#freeze
- Kernel#caller

詳しくは[mruby 1.3.0 released](https://mruby.org/releases/2017/07/04/mruby-1.3.0-released.html)をどうぞ。

## マルチオープン
URLのリストから複数のウェブページを開いたり、任意のアプリをまとめて起動することができます。

```ruby
URLS.each do |e|
  Browser.open e
end
```

以下の動画が分かりやすいと思います。

[https://twitter.com/ongaeshi/status/949177991237582848:embed]

私はこの機能を使ってクリップボードにコピーした行区切りの買い物リストをまとめてリマインダーに登録しています。超便利です。

## インストール
[App Store](https://itunes.apple.com/jp/app/rubypico/id1042498865)からどうぞ。

