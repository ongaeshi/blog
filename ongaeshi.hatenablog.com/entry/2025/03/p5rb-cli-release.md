---
Title: p5.rb CLIをリリースしました
URL: https://ongaeshi.hatenablog.com/entry/2025/03/p5rb-cli-release
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6802418398339418843
PreviewURL: https://ongaeshi.hatenablog.com/draft/entry/yb4CMcG5BXA9CgOwPQIWUVW_W84
Draft: true
---

p5rb_cli という RubyGems をつくりました。今まで p5.rb を使って Ruby でクリエイティブコーディングをしたい場合、ローカルに p5.rb スクリプトをダウンロードし、手動で読み込み用の HTML を作成する必要がありましたが、p5rb_cli を使うとコマンドを使って簡単にクリエイティブコーディングを始めることができます。

https://github.com/ongaeshi/p5rb_cli

# インストール
```
gem install p5rb_cli
```

Ruby さえあれば簡単にインストールできるように、最小限の gem 依存で作成しています。

# 使い方
## 新しいスケッチの作成

```bash
p5rb new path/to/hello.rb
# => Created hello.rb
```

作成された hello.rb は GitHub Copilot や Cursor がコードを生成しやすいように命名規則や代表的なメソッド名などがスクリプトのヘッダに添付されます。(不要なら削除しても問題ありません)

## スケッチの実行

```bash
p5rb run path/to/hello.rb
# => http://localhost:8000
```

指定したスケッチをローカルサーバーで実行し、ウェブアプリ経由でブラウザで表示します。

## ウェブアプリ
p5rb run で指定したスクリプトの実行結果が画面に表示されます。  
スクリプトを任意のエディタで編集してから Run ボタンを押すと画面が新しい実行結果で更新されます。  
Save ボタンを押すとスクリーンショットがローカルに保存されます。

![hello.rb](https://i.gyazo.com/768da33a6f56df6552a17f3b3b6776af.png)


# サンプル
## 1. 画像の表示
ローカル環境での実行なので [loadImage](https://p5js.org/reference/p5/loadImage/) などのローカルアセットにアクセスする API が使えるようになります。

![load_image.rb](https://i.gyazo.com/a9a88f4787c83c9fe73e25d6ff46a07d.png)

```ruby
def setup
  createCanvas(600, 600)
  @cat = loadImage('asset/cat.png')
  @landscape = loadImage('asset/landscape.jpg')
end

def draw
  background(255)
  image(@landscape, 0, 0)
  img_width = width / 5
  img_height = height / 5
  (0...1).each do |i|
    (0...5).each do |j|
      push
      translate(i * img_width + img_width / 2, j * img_height + img_height / 2)
      rotate((i - j) * (PI / 5))
      image(@cat, -img_width / 2, -img_height / 2, img_width, img_height)
      pop
    end
  end
end
```

## 3D

GitHub Copilot に手伝ってもらった書いた3Dデモです。（p5.js のこの辺りの API の知識は自分にはほとんどない状態で記述しています。）

![3d_primitive.rb](https://i.gyazo.com/2605980e80df04f759d4b5ab4614cf8f.png)]

```ruby
def setup
  createCanvas(710, 400, WEBGL)
  @voxel_colors = Array.new(11) { Array.new(11) { [random(255), random(255), random(255)] } }
end

def draw
  background(100)
  orbitControl

  noStroke()
  fill(50)
  push()
  translate(-275, 175)
  rotateY(1.25)
  rotateX(-0.9)
  box(100)
  pop()

  noFill()
  stroke(255)
  push()
  translate(500, height * 0.35, -200)
  sphere(300)
  pop()

  push()
  translate(0, -200, 0)
  rotateY(frameCount * 0.01)
  rotateX(frameCount * 0.01)
  beginShape()
  (0..9).each do |i|
    phi = map(i, 0, 9, 0, TWO_PI)
    (0..9).each do |j|
      theta = map(j, 0, 9, 0, PI)
      x = 10 * sin(theta) * cos(phi)
      y = 10 * sin(theta) * sin(phi)
      z = 10 * cos(theta)
      vertex(x, y, z)
    end
  end
  endShape(CLOSE)
  pop()

  push()
  rotateY(frameCount * 0.01)
  draw_voxel_mountain
  pop()
end

def draw_voxel_mountain
  size = 20
  (0..10).each do |i|
    (0..10).each do |j|
      h = noise(i * 0.1, j * 0.1) * 400
      push()
      translate(i * size - 100, -h / 2 + 100, j * size - 100)
      fill(*@voxel_colors[i][j])
      box(size, h, size)
      pop()
    end
  end
end
```

# まとめ
p5.rb がお気に入りのエディタやAI支援を受けながら開発することができるようになりました。

Ruby でのクリエイティブコーディングを楽しんでいただけたら幸いです。

