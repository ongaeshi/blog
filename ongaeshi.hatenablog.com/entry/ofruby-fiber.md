---
Title: ofrubyにFiber(コルーチン)を組み込んだ
Category:
- ofruby
- ruby
- mruby
- openframeworks
Date: 2015-05-26T00:46:52+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-fiber
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450095509622
---

ゲームとFiber(コルーチン)は相性がよいので組み込んだ。mrubygemsは使わずに直接組み込み。

[f:id:tuto0621:20150526004619g:plain]

- [ongaeshi/ofruby-ios@083e416](https://github.com/ongaeshi/ofruby-ios/commit/083e4168b7184fbfd48fe911be81cba1dad6c081)

PC環境でmruby使うならmrubygems便利だけど組み込み環境に小さなgem組み込むときは直接ソースコード追加した方が早い場合がある。

リリース前にライセンス表記には注意する必要あるけどmrubygemsでも結局組み込む必要があるのでそれはそんなに変わらないか。(CPANやRubyGemを使う場合はアプリケーション側にソースコード組み込む必要がないのが便利だったのだよね。今は緩いライセンスのライブラリ増えたし組み込んでもそんなに困ることは少ないけど)

## サンプルコード
こんな感じで使う。規則性のない動きが簡単に作れる。思考ルーチンなんかもコルーチン使うと大分書きやすい。

```ruby
# fiber.rb

def setup
  @rect = Rect.new
end

def update
  @rect.update
end

def draw
  @rect.draw
end

# ------------------------------
def wait(frame)
  1.upto(frame) { Fiber.yield }
end

class Rect
  attr_accessor :x
  
  def initialize
    @x = 0
    
    @fiber = Fiber.new do
      while true
      @x = 0
      wait 30
      
      @x = 50
      set_color_hex 0xc00000
      wait 30
      
      @x = 100
      set_color_hex 0x00c000
      wait 30
      
      @x = 150
      set_color_hex 0x0000c0
      wait 30
      
      @x = 200
      set_color_hex 0x000000
      wait 30
      
      @x = 250
      set_color_hex 0xffffff
      wait 30
      
      @x = 400
      wait 30
      end
    end
  end
  
  def update
    @fiber.resume if @fiber.alive?
  end
  
  def draw
    rect @x, 220, 80, 40
  end
end
```






