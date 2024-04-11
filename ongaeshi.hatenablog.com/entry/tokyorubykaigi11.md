---
Title: 東京Ruby会議11の感想(途中)
Category:
- ruby
Date: 2016-05-28T10:51:24+09:00
URL: https://ongaeshi.hatenablog.com/entry/tokyorubykaigi11
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653812171398528333
---

[Tokyo RubyKaigi 11 #tkrk11](http://regional.rubykaigi.org/tokyo11/)

HowTo禁止、楽しくハックする発表中心のカンファレンスというコンセプト最高です。質問時間も長めで大量の質問がなされていました。

## スケジュール
[スケジュール - Tokyo RubyKaigi 11 #tkrk11](http://regional.rubykaigi.org/tokyo11/schedule/)

## Streem
スピーカー: まつもとさん

[インタビュー](http://regional.rubykaigi.org/tokyo11/interview/matz/)
[matz/streem: prototype of stream based programming language](https://github.com/matz/streem)

言語作成には言語デザインに関する話題と言語実装に関する話題があるが、言語デザインに関する話題が少ない気がする。(まつもとさんは言語デザインの方が好き) 

- デザイン
  - 汎用言語を目指さない
  - 可能であればRubyと変える(Mustな部分とNot Mustな部分がある)
  - 20年の経験の反映
  - 関数型(的)プログラミング
  - イミュータブルデータ
  - コンカレンシー
  - ストリーミングプログラミング(ループがない, if文はある)

実例

```
stdin | stdout
```

```
["Hello World"] | stdout
```

```
# Simple echo server
tcp_server(8007) | each { |sock| sock | sock }
```

Streemを使って欲しい、というのもあるけどプログラミング言語を一からデザインすることの楽しさを伝えたかった。

プログラミング言語をデザインするのは楽しいよ。

## mruby/c
九州工業大学　田中 和明さん

[mrubyc/mrubyc](https://github.com/mrubyc/mrubyc)
[インタビュー](http://regional.rubykaigi.org/tokyo11/interview/kaz0505/)

mrubyよりももっと小さな環境で動かしたい。
OS不要、ドライバなしで動きます。

mrubyバイナリ。CPUに依存しない互換性あり。最初のRITEの4文字を見て、エンディアンやアライメントを理解しているらしい。

- GC
  - No GC
  - プログラムが終了するたびにメモリ初期化する。
- Concurrency
  - 複数のプログラムをVMが1命令ずつ交互に動かす
- Boot
  - 通常のRuby
    - VMの起動
    - クラスの作成
    - 全てのメソッドをクラスに登録
  - mruby/c はこれらのメソッド登録を実行まで遅らせている 

## Rubyに型があると便利か
spice life 栗原 勇樹 さん

[インタビュー](http://regional.rubykaigi.org/tokyo11/interview/ksss/)

型システム入門 - プログラム言語と型の理論 という本がよい。(でも難しい。)

### type_struct
[ksss/type_struct](https://github.com/ksss/type_struct)を作った。Keyword Arguemts が使える struct。

```ruby
Point = TypeStruct.new{
  x: Integer,
  y: Integer,
}
```

これ単体だと以外と使えなかった。

### TypeStructでJSONの型チェック
- Golang JSON
- Crystal JSON

のRuby版の機能をTypeStructに機能追加しては。JSONのValidateをTypeStructでかける。安心感がある。

ArrayやHashはArrayOfみたいな便利クラスを作った。

```ruby
class ArrayOf
  def initialize(type)
    @type = type
  end

  def ===
    self.all? { |e| e === @type }
  end
end
```

refinementsを使ったUnionの実装格好やInterfaceの実装もすごい。

### Ruby3の型システム
Ruby3の型について

- 静的チェック -> methodが呼べるかの事前check
- No Annotation
- Cでは頑張ってAnnotation書く

今後、静的解析に挑戦してみたい。型システム入門よりも簡単な本があったら教えてほしい。

matz : 型は書きたくないでござる(内部に型があるのはよい)
