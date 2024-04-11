---
Title: 今からObjective-C, iOSを勉強する時に気をつけること
Category:
- ios
- objective-c
- ruby
Date: 2014-08-11T02:46:46+09:00
URL: https://ongaeshi.hatenablog.com/entry/study-objective-c-and-ios
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815729997090
---

引き続きofrubyを作っています。

ファイルを管理する必要が出てきたので[FCFileManager](https://github.com/fabiocaccamo/FCFileManager)というライブラリを入れたら`EXC_BAD_ACCESS`エラーが出て半日位悩んだ。

検索しても分からずじまいで結局本屋に行って文献を漁ってやっと理解出来たのでメモ。

## iOSのリファレンスカウントの仕組みは途中で変わっている

- iOSのオブジェクト(NSなんとか)のメモリはリファレンスカウントで管理される
- 昔は retain, release, autorelease とか使って自前で管理する必要があった
- iOS5からARC(Automatic Reference Counting)という自動でリファレンスカウントのコードをコンパイラが生成してくれる仕組みが入った(すごい)
- ARCでは retain, release, autorelease は<b>書いてはいけない</b> (コンパイルエラーになる)

そして次が重要。

- <b>インターネットのサンプルコードはARC非対応、ARC対応のコードの両方が存在している</b>

コピペする時に混ぜこぜにすると大変なことになるので注意。

## openFrameworksのサンプルはARC非対応
ofrubyを作る時にopenFrameworksのサンプルコードから始めたのだけど、それはARC非対応だった(autoreleaseとか使って管理している)。

そこにARC対応だった[FCFileManager](https://github.com/fabiocaccamo/FCFileManager)のコードをコピーしたのでさあ大変。リファレンスカウントが上手く動かずエラーになってしまった、ということだったようだ。後でREADMEを見返したら`iOS >= 5.0`, `ARC enabled`ってちゃんと書いてあるね・・。

対策としてはofrubyのコードをARC対応に書き換えることで上手く動くようになった。幸いまだコードも少なかったし、コンパイルスイッチやXcodeの設定で簡単に変更出来た。リファクタリングツールのサポートもあるようので今から書くコードは積極的にARCを使うようにしたい。

Appleが頑張っている点として<b>リンク単位ではARC対応、ARC非対応のコードが混在出来る</b>ことが挙げられる。今回も自分の書いたプロジェクトはARC対応に書き換えたが、openFrameworksのライブラリプロジェクトはARC非対応のままで上手く動かすことが出来た。

## 役に立った本

[asin:4797368276:detail]

の<b>CHAPTER 5</b>にとても詳しく書いてあります。Swiftが使えるようになったとしてもCocoaのコアライブラリ自体はObjective-Cで書かれているので今から勉強してもそんなに損は無いと思っています。

[asin:4897978440:detail]

今回の本に関係ないけど何か作りたいならこっちの本もよい。インタフェースビルダーを使わない所とか素敵。

## 今日の成果
Objective-C 2.0 第3版 のおかげでバグも直り、ファイルの保存、保存したファイルの実行まで出来ました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140811/20140811024615.png" alt="f:id:tuto0621:20140811024615p:plain" title="f:id:tuto0621:20140811024615p:plain" class="hatena-fotolife" itemprop="image"></span></p>
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140811/20140811024621.png" alt="f:id:tuto0621:20140811024621p:plain" title="f:id:tuto0621:20140811024621p:plain" class="hatena-fotolife" itemprop="image"></span></p>
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140811/20140811024627.png" alt="f:id:tuto0621:20140811024627p:plain" title="f:id:tuto0621:20140811024627p:plain" class="hatena-fotolife" itemprop="image"></span></p>











