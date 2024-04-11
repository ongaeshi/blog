---
Title: ofruby開発日誌(1) - タッチ操作の組み込み
Category:
- ruby
- ofruby
- mruby
- openframeworks
- cpp
- C++
Date: 2014-09-12T00:35:08+09:00
URL: https://ongaeshi.hatenablog.com/entry/ofruby-touch-point
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815732581662
---

ofrubyのダウンロードはこちらからどうぞ。

<a href="https://itunes.apple.com/us/app/ofruby/id908715098?mt=8&uo=4" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a>

ofruby0.2(審査待ち→<b>今朝通過しました</b>)でタッチした複数の位置情報を取得出来るようになりました。

```ruby
module Input
  # タッチ位置を取得(0〜4)
  def self.touch(idx)

  # 全てのタッチ位置をArrayで取得
  def self.touches
end

class TouchPoint
  # タッチしているか？
  def valid?

  # x座標
  def x
  
  # y座標
  def y
  
  # 押した瞬間にtrue
  def press?

  # 押しているか？(valid?と同じ) 
  def down?

  # 離した瞬間にtrue
  def release?
end
```

内部でC++で管理しているタッチ情報をRubyのArrayで保持するようにしたのですが、上手くいってかなりすっきりした実装になりました。

範囲外アクセスはArrayが勝手にやってくれますし、

```ruby
Input.touch(10) # Arrayの範囲外エラー
Input.touch(-1) # 同じく
```

Input.touchesを使うとRubyらしい表現を使ってタッチ位置を処理出来るようになります。美しいですね。

```ruby
# 最初に見つかったタッチ位置を返す(見つからない時はnilを返す)
t = Input.touches.find { |t| t.valid? }

# タッチしているタッチ点のみを抜き出す
touches = Input.touches.find_all { |t| t.valid? }

# 2点以上タッチしているか？
if touches.size >= 2
  # 中点を求める
  center_x = touches.reduce(0) { |a, e| a + e.x } / touches.size
  center_y = touches.reduce(0) { |a, e| a + e.y } / touches.size
end
```

入力を取ることが出来れば作れるものの幅がかなり広がります。早く審査が通って欲しい所です。

