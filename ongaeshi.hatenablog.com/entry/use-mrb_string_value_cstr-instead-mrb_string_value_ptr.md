---
Title: mrubyの文字列をC言語から参照するときはmrb_string_value_ptr()ではなくmrb_string_value_cstr()を使う
Date: 2018-01-06T14:34:24+09:00
URL: https://ongaeshi.hatenablog.com/entry/use-mrb_string_value_cstr-instead-mrb_string_value_ptr
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812334076940
---

```ruby
src = Clipboard.get.split("\n")

src.each do |e|
  p e
  puts e
  puts
end
```

みたいなコードを書いているときに、`p`だと正しく表示されるけど`puts`だと後ろの文字列が一緒に表示されてしまうときがあった。

```
# クリップボードの内容は"foo\nbar\nbaz\n"とする
"foo"
foo
bar
baz

"bar"
bar
baz

"baz"
baz
```

調べていくと、C言語内でmrubyの文字列を参照しているときに`mrb_string_value_ptr()`を使っているのが原因だった。NULL終端された文字列が確実に欲しいときは`mrb_string_value_cstr()`を使う必要がある。

- [Make sure that mrb_string_value_ptr() returns a null-terminated string. by sdottaka · Pull Request #2673 · mruby/mruby](https://github.com/mruby/mruby/pull/2673)
- [use mrb_string_value_cstr() instead of mrb_string_value_ptr() · Issue #9 · iij/mruby-env](https://github.com/iij/mruby-env/issues/9)

慌ててRubyPico全体をgrepして、ひととおり`mrb_string_value_cstr()`に変更した。結構あったのに、意外とちゃんと動いていてびっくり。

- [Use mrb_string_value_cstr() instead of mrb_string_value_ptr() · ongaeshi/RubyPico@f1a1264](https://github.com/ongaeshi/RubyPico/commit/f1a1264c34af4df4b0b2e1dbb9b96f0e8c7acbc2)

後で[SketchWaltz側も直して](https://github.com/ongaeshi/sketchwaltz/search?utf8=%E2%9C%93&q=mrb_string_value_ptr&type)おこう。

※ このバグは申請中のRubyPico 0.9.7 で直ります


