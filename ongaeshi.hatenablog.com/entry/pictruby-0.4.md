---
Title: iOSのRubyプログラミング環境 PictRuby 0.4 - p, puts が使えるように
Category:
- rubypico
- mruby
- ruby
- ios
Date: 2016-02-21T21:49:30+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-0.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792364203892
---

[PictRuby - Ruby Programming Environment in iOS](http://pictruby.ongaeshi.me/)

0.4にバージョンアップしました。

- p, puts が使えるように
- スクリプト名の変更が可能に

## p, puts が使えるように
p, puts を使ってテキストを出力できるようになり、CRubyに少し近づきました。九九テーブルを出力するプログラムも以下のように簡単に書けるようになります。

```ruby
# # kuku

def main
  puts "九九 🙂😌🙄😨😢😩😬😁😄"
  puts
  
  1.upto 9 do |a|
    1.upto 9 do |b|
      printf "%3d", a * b
    end
    puts
  end
end
```

絵文字も簡単にputsできるのはスマホのいいところですね・・。

[f:id:tuto0621:20160221125338p:plain]

### 0.3 以前のコードの変更方法
今までテキストを返していたコードはputsを使う必要があります。(画像を返すときは今までどおりです)

```ruby
# Before 0.3
def main
  "Hello, PictRuby"
end
```

```ruby
# After 0.4
def main
  puts "Hello, PictRuby"
end
```

## スクリプト名の変更が可能に
ファイル名を直接変更できるようになりました。

[f:id:tuto0621:20160221125113g:plain]

## インストール
[https://itunes.apple.com/jp/app/pictruby/id1042498865?mt=8&uo=4:embed]

## 過去の記事
- [iOSのRubyプログラミング環境、PictRuby 0.3 をリリースしました](http://ongaeshi.hatenablog.com/entry/pictruby-0.3)

