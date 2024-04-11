---
Title: ruby.wasm の JS::Object のプロパティ呼び出しをさらに便利にする
Date: 2023-02-19T21:47:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/access_properties_from_jsobject_in_function_style
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/4207112889964644501
---

ruby.wasm でいろいろ遊んでいます。実験場も作りました。

[https://rubywasm-sample.ongaeshi.me/:embed:cite]

JS::Object が楽しくて、JSのオブジェクトをRubyから透過的に扱えます。メソッド呼び出しもできるしブロックで関数を渡せたしりします。

```ruby
require 'js'
JS.eval("return 1 + 2") # => 3
JS.global[:document].write("Hello, world!")
div = JS.global[:document].createElement("div")
div[:innerText] = "click me"
JS.global[:document][:body].appendChild(div)
div.addEventListener("click") do |event|
   puts event          # => # [object MouseEvent]
   puts event[:detail] # => 1
   div[:innerText] = "clicked!"
end
```

後はプロパティへの読み書きが `JS.global[:document]`を経由しないといけないのがちょっと難しいのですが、これも method_missing を頑張ればなんとかならないかと色々試していました。

## JS::Object でプロパティに対しても関数スタイルで呼べるようにする
[本家](https://github.com/ruby/ruby.wasm/blob/main/ext/js/lib/js.rb#L106-L117)の method_missing にもう1パッチ当てました。

ハッシュにアクセスしたときに "function", "undefined" 以外が見つかったときはそのまま JS::Object として返します。また `:foo=` が来たときはまず `self[:foo]` で JS::Obejct を見つけたのち `[:foo] = args.first` で代入します。

```ruby
require "js"

class JS::Object
  def method_missing(sym, *args, &block)
    ret = self[sym]

    case ret.typeof
    when "undefined"
      str = sym.to_s
      if str[-1] == "="
        self[str.chop.to_sym] = args.first
        return args.first
      end

      super
    when "function"
      self.call(sym, *args, &block).to_r
    else
      ret.to_r
    end
  end

  def respond_to_missing?(sym, include_private)
    return true if super
    self[sym].typeof != "undefined"
  end
end
```

## JS::Object が数値や文字列のときは Ruby オブジェクトに変換する
上で大体いいのですが、返された JS::Object をさらにRubyオブジェクトと計算しようとするとエラーになります。

```ruby
p "foo".length + 3   # Error: JS::Object + Integer は未定義
```

そこで JS::Object#to_r というメソッドを追加して数値や文字列のときはRubyの数値や文字列オブジェクトに変換してしまいます(よく見ると↑のサンプルでも関数コールやプロパティgetterの後ろで to_r を呼んでいます)

※ 本当は整数と少数に分けて変換したいところだけど JavaScript は全て number で来るのでひとまず to_f に統一・・

```ruby
class JS::Object
  def to_r
    case self.typeof
    when "number"
      self.to_f
    when "string"
      self.to_s
    else
      self
    end
  end
end
```

## おわり
プロパティもJSと同じ感じでアクセスできるようになり、かなりよくなったのではないでしょうか？

[https://rubywasm-sample.ongaeshi.me/access_properties_from_jsobject_in_function_style/]

```ruby
require 'js'
JS.eval("return 1 + 2") # => 3
JS.global.document.write("Hello, world!")
div = JS.global.document.createElement("div")
div.innerText = "click me"
JS.global.document.body.appendChild(div)
div.addEventListener("click") do |event|
   puts event          # => # [object MouseEvent]
   puts event.detail # => 1
   div.innerText = "clicked!"
end
```
