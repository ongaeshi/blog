---
Title: gifアニメにお絵描きできるGifDrawerをリリースしました
Date: 2022-03-12T17:16:48+09:00
URL: https://ongaeshi.hatenablog.com/entry/gifdrawer
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/13574176438070580162
---

リモートワークでスクショやgifアニメに手書きコメントを付けて共有する機会が増えたのですが、
手書きコメントにもアニメーションがつけられたら便利なんじゃないかと思い作りました。

<div class="images-row mceNonEditable">[f:id:tuto0621:20220308011501g:plain:alt=左クリックで書く][f:id:tuto0621:20220308011626g:plain][f:id:tuto0621:20220308011651g:plain]</div>
<div class="images-row mceNonEditable">[f:id:tuto0621:20220308012355g:plain][f:id:tuto0621:20220308011726g:plain]</div>
<div class="images-row mceNonEditable">[f:id:tuto0621:20220308011740g:plain][f:id:tuto0621:20220308011752g:plain]</div>

## インストール
[https://github.com/ongaeshi/GifDrawer]の[tags](https://github.com/ongaeshi/GifDrawer/tags)から最新版をダウンロードして適当なところへ展開してexeをダブルクリックすれば動きます。gitレポジトリを直接cloneしてもよいです。

お絵描きしながらタイムラインを動かすことでアニメーションがつけられます。コマ送りを使うと書きやすいです。ペンタブで書きたい人はWindows InkをOFFにしてください。

gifや画像をドラッグ＆ドロップするとそれを背景にしてお絵描きできます。

## ソフトウェア構成
- [ClipScript](https://github.com/ongaeshi/ClipScript)
- [OpenSiv3D](https://github.com/Siv3D/OpenSiv3D)
- [mruby](https://github.com/mruby/mruby/) (using [mruby-packer](https://github.com/ongaeshi/mruby-packer))

ClipScriptというタイムラインに連動したアニメーションをスクリプトで記述できるアプリケーションの上で作っています。
ClipScriptはOpenSiv3DのAPIをmrubyにバインドして動いています。

大部分がスクリプトで動いているためmain.rbのパラメータを変更すると色セットやペンの太さを変更することができます(本当はもっと色々できます)。

```ruby
# 調整用パラメータ

# ペンの色
# "navy", "blue", "aqua", "teal", "olive", "green", "lime", "yellow", "orange"
# "red", "fuchsia", "purple", "maroon", "white", "silver", "gray", "black"
PEN_COLORS = ["red", "blue", "green", "black"]

# ペンの太さ
PEN_THICKNESSES = [1, 2, 4, 8]

# 消しゴムの太さ
ERASER_THICKNESS = 32

# gifアニメが未設定のときの終了時間
DEFAULT_END_TIME = 3

# コマ送りの再生レート(1が60fps、3で20fps)
FRAME_ADVANCE_RATE = 3

.
.
```

## おわりに
感想や質問など #gifdrawer ハッシュタグを付けたりしてつぶやいてもらえたら嬉しいです。

