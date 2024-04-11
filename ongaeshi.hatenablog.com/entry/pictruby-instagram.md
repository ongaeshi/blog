---
Title: PictRubyでインスタグラム風レイアウトを実現する
Category:
- ruby
- mruby
- picture
- rubypico
- 写真
Date: 2015-11-09T23:54:48+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-instagram
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653458415127538602
---

先日リリースした[PictRuby](http://ongaeshi.hatenablog.com/entry/release-pictruby)を使ってInstagram風の横並べグリッド画像を作ってみます。

[https://twitter.com/instagram/status/663400124434006016:embed#The Week on Instagram | 208 https://t.co/S5Bf5ok5wv https://t.co/xwFLrYvLsj]

インストールはこちらからどうぞ。

<p><a href="https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=1042498865&amp;mt=8" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a></p>

## インストール

[gist034fb1fd4fb737127ab5](https://gist.github.com/034fb1fd4fb737127ab5)

[https://gist.github.com/034fb1fd4fb737127ab5:embed#gist034fb1fd4fb737127ab5]

1. このソースコードをクリップボードにコピーします([view raw](https://gist.githubusercontent.com/ongaeshi/034fb1fd4fb737127ab5/raw/04910aeaa93fc8bdcc5c8e46197fbdaa4b2ab7b9/instagram.rb)が便利です)
2. [PictRuby](https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=1042498865&amp;mt=8)を起動します
3. '+'ボタンを押して新しいファイルを作ります
3. ソースコードを貼り付けて'Run'ボタンを押します
4. アルバムから好きな写真を9枚指定します

## 実行結果

このような9枚の写真が・・

[f:id:tuto0621:20151109235123j:plain:w200]

こんな感じにレイアウトされます。(元画像はサイズバラバラでも自動で正方形に切り出して綺麗にレイアウトしてくれます)

[f:id:tuto0621:20151109235136j:plain]

## ソースコード解説

```ruby
# Please return the Image object in the

def convert
  # アルバムから写真を9枚取得
  imgs = Image.pick_from_library(9)   
  
  ImageUtil.horizontal([
      # 最初の画像を正方形                               
      imgs[0].square,                     
      # 2〜5枚目を全て正方形にして、2x2のグリッド画像に       
      ImageUtil.grid(imgs[1..4].map { |e| e.square }),  
      # 6〜9枚目を全て正方形にして、2x2のグリッド画像に       
      ImageUtil.grid(imgs[5..8].map { |e| e.square })
  ])  # 作った画像を横に並べる
end
```

結果として

```
 -----------------
 | 1 | 2x2 | 2x2 |
 -----------------
```

な横長画像が完成します。応用で

```
 -----------------
 | 2x2 | 1 | 2x2 |
 -----------------
```

など色々なレイアウトが作れます。
