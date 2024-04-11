---
Title: 最近作っているClipScript
Date: 2021-06-19T00:45:21+09:00
URL: https://ongaeshi.hatenablog.com/entry/developping-clip-script
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/26006613777429819
---

https://github.com/ongaeshi/clipscript

短い動画を簡単に作成するためのスクリプト言語です。

[f:id:tuto0621:20210618235722g:plain]

```ruby
require 'clip'

App.window_size(400, 225)

font_s = Font.new(40)
font = Font.new(50)
smile = Texture.new(Emoji.new("😀"))

script do |root|
  Drawer.background "white"

  (0..10).each do |x|
    (0..10).each do |y|
      if (x + y) % 2 == 0
        root.rect(x * 40, y * 40, 40, 40, color: "gray")
        root.wait 0.02
      end
    end
  end
end  

script do |root|
  t = root.text(font, 200, 100, color: "black", text: "Hello, World!", length: 0, center: true)
  root.wait 0.2

  1.upto(t.text.length) do
    t.length += 1
    root.wait 0.1
  end

  root.until_time 3

  x = root.texture(smile, 180, 150)
  x.scale(0.4, 0.4)
end

App.run
```

## タイムラインUI
記述したスクリプトは好きな場所から再生したり逆再生することができます。

[f:id:tuto0621:20210619000458g:plain]

## gifアニメを再利用した動画の作成
gifアニメの再生機能に力を入れており、動画ファイルの編集無しで以下のようなことができます。

- テロップの表示
- 再生レートの変更
- サイズの調整

[f:id:tuto0621:20210619004049g:plain]

これは直前に紹介したgifファイルを再利用して作成した動画です。追加で書いたのは[このスクリプト](https://github.com/ongaeshi/TechGif/blob/main/introduction.rb)のみです。

## インストール
最新版をGitHub Actionでビルドしたものがあるので興味がある人は触ってみてください。

1. https://github.com/ongaeshi/ClipScript/actions から(成功した)最新のActionを開いてbuild-resultをダウンロード
2. .rbファイルを.exeにドラッグ＆ドロップすると動きます

## 使用ライブラリ
- OpenSiv3D
- murby

OpenSiv3dは[gifアニメの再生が簡単](https://siv3d.github.io/ja-jp/news/v043/#3-gif)なのがよいです。(次の0.6ではmp4の再生もできるようになるそうなのて大変楽しみです。)

mrubyは自作の[ongaeshi/mruby-packer](https://github.com/ongaeshi/mruby-packer)を使ってソースコード丸ごとレポジトリに入れてあります。(ので、ソースコードを対応するプラットフォームのOpenSiv3DにコピーすればMacやLinuxでも動くはず)
