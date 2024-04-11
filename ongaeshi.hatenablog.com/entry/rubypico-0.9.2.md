---
Title: RubyPico 0.9.2 リリース - Appタブ、irb、Browser.post、choise
Category:
- RubyPico
Date: 2016-12-01T01:21:54+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.9.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687196783985
---

RubyPico 0.9.2 をリリースしました。irbをエディタ画面を経由せず実行できるようになったり、自分の作ったスクリプトをアプリとして登録できるようになりました。

他にもPOSTメソッドを呼び出せる`Broser.post`や、複数選択肢の中からタップさせて結果を返してくれる`choise`など楽しいAPIがたくさん組み込まれました。

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## Appタブ
[f:id:tuto0621:20161201001242p:plain]

今まであった File, Sample に加え、新たに<b>App</b>タグが増えました。ここに表示されたスクリプトは<b>エディタ画面を経由せずに直接実行されます</b>。スクリプトの中身を確認したいのではなく実行した結果に興味があるときは大変便利です。

irb, lineno が標準でアプリ化されています。特にirbは動作を確認したいときや簡易電卓として頻繁に使う人が多いのではないかと思います。

## 自作スクリプトをアプリとして登録する
[f:id:tuto0621:20161201001321p:plain]

自分で書いたスクリプトも簡単にアプリ化することができます。ルートディレクトリに`.app/`というフォルダがあるのでその中に置かれたファイルが自動的にアプリタブに表示されます。irbやlinenoが不要な場合は消すことも可能です。

[f:id:tuto0621:20161201001419p:plain]

.app/内のスクリプトは`.rb`が付いていませんが、普通のRubyスクリプトとして実行されます。もちろん直接プログラムを書いてもよいのですが、プログラム本体は別の場所におき`.app/`以下はrequireだけを呼ぶようにしておくのがおすすめです。

[f:id:tuto0621:20161201001428p:plain]

```ruby
require 'sample/irb'
```

RubyPicoの特殊ルールとして`require 'sample/xxx'`と呼ぶとSampleタブにあるコードをrequireすることができます。それ以外はルートディレクトリからの絶対パスでrequireしたいファイルを指定します。例えば`/foo/bin/main.rb`をAppタブに置きたい場合は以下のように書きます。

※ 新たに追加された`Copy`ボタンを使って他のアプリを流用すると簡単です

```ruby
# .app/foo に置く
require 'foo/bin/main'
```

Appタブを使うと自分や他の人の作ったスクリプトを簡単に呼び出すことができます。また.app/以下にファイルを作るだけなのでインストール自体もRubyスクリプトで書くことができます。面白い機能なので是非使ってみてください。

## Browser.post
今まで`Browser.get`や`Browser.json`を使ってGETメソッドを送ることはできていましたが、POSTメソッドも送信できるようになりました。以下のようなことができるようになります。

- [Gistに自分の書いたコードを保存](http://ongaeshi.hatenablog.com/entry/2016/11/13/005602)
- [LINE Notifyの送信](http://ongaeshi.hatenablog.com/entry/2016/11/14/235638)

## choise
ちょっと不思議なメソッドです。文字入力なしでユーザーが選択できるようになるためスマホらしいUXが作れるのではないかと期待しています。

- 選択肢を配列に格納して`choise`を呼び出し
- リンク文字列として出力され<b>なにかがタップされるまでプログラムが停止</b>する
- タップされたらプログラムはふたたび動き出す、タップした文字列を結果として返す

以下はサンプルコードです。提示された選択肢をタップしていくことでプログラムが進みます。(アドベンチャーゲームとか作れそうです)

```ruby
puts "What your birthday?"
r = choise(%w(Jan Feb Etc))

if r != "Etc"
  puts
  puts "Hello, #{r}"
  return
end

puts "Sorry"
r = choise(%w(Mar Apr Jun Jul Etc), with_no: true)

if r!= "Etc"
  puts
  puts "Hey, #{r}"
  return
end

puts "Oh, Sorry"
r = choise(%w(8 9 10 11 12), combine: true)
puts 
puts "Yah, #{r}"

r = choise(%w(1 2 3).map { |e| e + "\n" })
puts r
```

実行結果です。

[f:id:tuto0621:20161201001530p:plain]

## インストール
[App Store](https://itunes.apple.com/jp/app/rubypico/id1042498865)からどうぞ。

## おまけ
個人開発者 Advent Calendar 1日目に参加しました。

- [RubyPico - スマホで楽しくプログラミングできるRuby開発環境 - Qiita](http://qiita.com/ongaeshi/items/c8400fa49eaeaa307f90)
