---
Title: PictRubyでスマホプログラミング - 今平成何年？
Category:
- rubypico
- ruby
- ios
Date: 2016-02-04T19:00:40+09:00
URL: https://ongaeshi.hatenablog.com/entry/heisei
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347155356399
---

今が平成何年かを表示します。

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## ソースコード

以下をコピペしてPictRubyに貼り付けてください。

[get_sample.rb](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)を使って`ja/heisei`でも取得するとこができます。

```ruby
# # ja/heisei
#
# ## 概要
# 今が平成何年か表示します。

def main
  t = Time.now
  h = t.year - 1989 + 1
  "平成#{h}年#{t.month}月#{t.day}日"
end
```

## 使い方
実行すると今の和暦を表示します。

[f:id:tuto0621:20160131170405p:image:w300]

スクリプトは[ランチャーアプリ経由](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)で通知センターに登録しておくとさらに便利です。


