---
Title: RubyPico開発日誌4 - getsでirbを再実装
Category:
- rubypico
Date: 2016-09-24T00:52:39+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-diary-4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687185927026
---

コンソールを表示しながら入力できるようになったので、irbをgetsで再実装してみた。

```ruby
# # irb
#
# ## Description
# Interactive Ruby Shell (REPL).

puts "irb - Interactive Ruby Shell"
no = 0

loop do
  print "irb:%03d> " % no
  cmd = gets
  
  puts cmd
  
  break if cmd == "exit"
  
  begin
    puts "=> #{eval(cmd).inspect}"
  rescue Exception => e
    puts e.message
  end
  
  no += 1
end
```

実行画面。pがちゃんとCRubyと同じ挙動になったぞ万歳。(コンソールに出力しつつ、戻り値も返す)

[f:id:tuto0621:20160924004944p:plain]

文字に色付けたくなるね。
