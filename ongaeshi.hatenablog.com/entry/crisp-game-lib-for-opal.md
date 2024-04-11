---
Title: 残りゲーム体力が少なくてもブラウザゲームをRubyで簡単に書きたい
Date: 2022-01-10T21:39:31+09:00
URL: https://ongaeshi.hatenablog.com/entry/crisp-game-lib-for-opal
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/13574176438051344367
---

[crisp-game-lib](https://github.com/abagames/crisp-game-lib)というゲーム制作体力が残り10%でもゲームが書けるJavaScriptのライブラリがあるのだが、それを[Opal](https://opalrb.com/) に移植してRubyで書けるようにした。(画面クリックで遊べます)

[https://ongaeshi.github.io/crisp-opal-test/fall/:image=https://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20220110/20220110173735.jpg]

2色のりんご🍎が落ちてくるので、同じ色を取ると得点、違う色は避ける、画面タップでプレイヤーの色が変わるので状況によって使い分けよう。モバイルの場合は両手プレイで移動と色替えの操作の手を分けると遊びやすいかも。

## コード
https://github.com/ongaeshi/crisp-opal-test/blob/master/fall/main.rb

```ruby
setup(
  title: "FALL",
  description: <<~EOS,
    [SWIPE] Move
    [TAP] Color
  EOS
  characters: [
    <<~EOS,
      llllll
      ll l l
      ll l l
      llllll
       l  l
       l  l
    EOS
    <<~EOS,
       pp
       pp
     rrrrrr
     rrrrrr
     rrrrrr
     rrrrrr
    EOS
    <<~EOS,
       yy
       yy
     llllll
     llllll
     llllll
     llllll
    EOS
  ],
  options: {
    isPlayingBgm: true,
    isReplayEnabled: true,
    seed: 200
  }
)
 
class Falling
  def initialize(string, x, y)
    @string = string
    @pos = vec(x, y)
  end

  def update(player_color)
    @pos.y += 1
  
    if char(@string, @pos).is_colliding.char.a
      if player_color == "red" && @string == "b" ||
        player_color == "black" && @string == "c"
        add_score(1, @pos)
        return true
      else
        end_game
      end
    end

    @pos.y > 102
  end
end

@player_color = nil
@fallings = nil

def update
  if ticks == 0
    @player_color = "red"
    @fallings = []
  end

  if input.is_just_pressed
    @player_color = @player_color == "red" ? "black" : "red"
  end

  color(@player_color)
  char("a", input.pos.x, 95)

  color("black")
  @fallings.push(Falling.new(rndi(2) == 1 ? "b" : "c", rnd(100), 0)) if ticks % 5 == 0
  @fallings.delete_if do |e|
    e.update(@player_color)
  end
end
```

crisp-game-libは`update`という関数の中に処理を書いてゲームが作れる。初期化処理は`if ticks==0`の中に書く。

crisp-game-libの特徴として描画処理と当たり判定が一体化しているところがある。対象と重なっているかは描画したときの戻り値を使って判定する(まだ描画されていない対象との当たり判定は当然取得できないので描画順に気をつける必要がある)。これは極力短く書くための仕組みで、ゲームは通常ロジックと描画が分かれて記述することが多いが確かにこれならコード量が半分になり余計なことを考えずに済むと思った。テクスチャも貼れない(ドットは配列作れば打てる)、サウンドは自動生成されたものを番号指定するだけ、効果音組み込みで数種類の中から選択、とライブラリ全体としてゲームを高速に作るために特化していて書いていると心地よい。(このゲームも書き始めてから30分位で書けたと思う) 後リソースを一切要求しないのでコードだけで配布できるのもよい。

もっとたくさんのゲームを触ってみたい方は、crisp-game-lib作者のABAさんが[すでに大量の面白いゲーム](https://github.com/abagames/111-one-button-games-in-2021/blob/main/README.md#i-have-created-111-one-button-games-in-2021)を作られているので是非遊んでみて欲しい。ほぼ全てのAPIがOpalに移植できていると思うのでブラウザゲームをRubyで記述したい、という趣味の方がいましたら是非お使いください。

