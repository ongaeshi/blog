---
Title: Pocketに保存したページをランダムに3つ表示してくれる「Iriguti」をリリースしました
Category:
- ruby
- rails
- web
- pocket
- webアプリ
Date: 2014-07-17T01:19:10+09:00
URL: https://ongaeshi.hatenablog.com/entry/iriguti-released
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815728331707
---

Pocketに保存したページをランダムに3つ表示してくれる「Iriguti」というWebアプリを作りました。

[Iriguti](http://iriguti.ongaeshi.me/)

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140717/20140717011401.png" alt="f:id:tuto0621:20140717011401p:plain" title="f:id:tuto0621:20140717011401p:plain" class="hatena-fotolife" itemprop="image"></span></p>


## 作った動機
[Ruby on Rails チュートリアル](http://railstutorial.jp/)を読み終えたのでせっかくなのでRailsで何か作りたいなぁと思っていました。ある時、たまったまま消化出来ないPocket未読記事と、1日に何回も同じニュースサイトにアクセスしていることに気がつきました。Pocketには読みたい記事がいっぱいたまっているのに、なぜ何回も同じニュースサイトにアクセスしてしまうのでしょうか？

1つ目の理由として「Pocketにストックされている記事の中から1つを選び出すのは割とコストが高い」ということが挙げられます。せっかくストックした記事なのでちゃんと読みたい。でも今はがっつり読む時間が無い。だからさくっと読める軽いニュースを・・ということが私はよくあります。

2つ目の理由として「ニュースサイトはアクセスする度に変化する」ということが挙げられます。もしかしたら何か面白い発見があるかもしれないという期待がアクセスを後押しするのです。

これらの問題を解決するために、見に行くたびにPocket記事をランダムに3つ表示することで選ぶコストを軽減し、ニュースサイトのようにちょっとだけ楽しみが持てれば消化が進むのではないかと思い作り始めました。

## 使い方
Pocketアカウントでサインインします。
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140717/20140717011656.png" alt="f:id:tuto0621:20140717011656p:plain" title="f:id:tuto0621:20140717011656p:plain" class="hatena-fotolife" itemprop="image"></span></p>


ログインするとPocketにアーカイブされた記事のなかからランダムに3つ表示されます。
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140717/20140717011707.png" alt="f:id:tuto0621:20140717011707p:plain" title="f:id:tuto0621:20140717011707p:plain" class="hatena-fotolife" itemprop="image"></span></p>

リロードすると別のページが表示されます。
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140717/20140717011717.png" alt="f:id:tuto0621:20140717011717p:plain" title="f:id:tuto0621:20140717011717p:plain" class="hatena-fotolife" itemprop="image"></span></p>

チェックボタンでページのアーカイブ/再ストックが出来ます。
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140717/20140717011727.png" alt="f:id:tuto0621:20140717011727p:plain" title="f:id:tuto0621:20140717011727p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## Irigutiのある1日
1. 朝(夜)起きたら、ニュースサイト、はてブ、RSSなどから読みたい記事をPocketにクリップします。(1日に1〜2回程度)
2. 以後ニュースサイトへのアクセスは控えましょう
3. 暇になった時は[Iriguti](http://iriguti.ongaeshi.me/)にアクセスします
4. 3つの中からピンときた記事をクリックします
5. 読み終わったらチェックボタンをクリックしましょう。読み終わらなかった時はそのままで構いません(後述)。
6. 飽きるまで3〜6を繰り返します。

## よい記事は何度も読み返す
よい記事やページは読み応えがあります。一度読んだだけでは内容や作者の意図を汲み取れないかもしれません。しかし、そういった記事はあなたにとってよい刺激を与えてくれる可能性が高いです。

諦めずに一度に1節、1文ずつでいいのでちょっとずつ進んでいきましょう。[Iriguti](http://iriguti.ongaeshi.me/)を使っている場合はチェックボタンをクリックしなければそのうちまたリロード時に出てくるので適度に間隔を空けながら読んでいくといいです。

最近、私自身は[正確な文章の書き方](https://gist.github.com/ongaeshi/85ba6bf920f8326542c9)や[C#/.NETがやっていること 第二版](http://www.slideshare.net/ufcpp/cnet-36422788)をちょっとずつ読み進めています。

是非[Iriguti](http://iriguti.ongaeshi.me/)を使った皆さんの感想を聞かせて下さい。ジャンクテキストの波に溺れないように楽しいインターネットライフを。
