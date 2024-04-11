---
Title: PictRubyでスマホプログラミング - 筋トレをWorkflowyに記録する
Category:
- rubypico
- ruby
- ios
Date: 2016-01-29T04:07:19+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-mascle
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347154838479
---

日々のスクワットや腕立て伏せの回数を簡単に記録できるスクリプトです。筋トレした日付を任意のフォーマットで自動入力したくて作りました。

[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## ソースコード

以下をコピペしてPictRubyに貼り付けてください。

```ruby
# mascle.rb

def main
  squat = Popup.input "スクワット？"
  pushup = Popup.input "腕立て伏せ？"
  
  t = Time.now
  date = sprintf("%04d-%02d-%02d", t.year, t.month, t.day)
 
   Clipboard.set "スクワット #{squat} 腕立て伏せ #{pushup} #d-#{date}"
   
  # ここは自分の記録したいページやアプリに変更
  Browser.open "https://workflowy.com/#/hogehoge"
end
```

## 使い方
起動すると各トレーニングの回数を聞かれます。

[f:id:tuto0621:20160129035653p:image:w300]

入力するとクリップボードに今日の日付付きのテキストがコピーされます。Workflowyの記録用ページが開くのでクリップボードの内容を貼り付けて終了です。

[f:id:tuto0621:20160129035659p:image:w300]

スクリプトは[ランチャーアプリ経由](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)で通知センターに登録しておくとさらに便利です。

