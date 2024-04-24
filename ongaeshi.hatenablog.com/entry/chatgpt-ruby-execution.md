---
Title: ChatGPT に Ruby など他のプログラミング言語も実行させたい
Category:
- ruby
- chatgpt
- programming
Date: 2024-02-05T15:00:00+09:00
URL: https://ongaeshi.hatenablog.com/entry/chatgpt-ruby-execution
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189080266145
---

[f:id:tuto0621:20240204225423p:plain]

ChatGPT はたくさんの種類のプログラミング言語のコードを出力できますが、直接実行できるのは現状 Python のみとなっています。Ruby など他の言語も実行することはできないかと考えていました。

1つのアイデアとして思いついたのは ChatGPT に生成したコードを [Ruby on Browser](https://rubyonbrowser.ongaeshi.me) に渡す URL を生成してもらい、それをクリックするだけで実行できるようにするという方法です。これなら他の言語でも URL 経由で実行できる処理系なら応用できそうです。(現状は ChatGPT 4 でのみ実行可能な模様)

# 下準備
まず Ruby on Browser に URL 経由でコードを渡す方法を用意する必要があります。もともとクエリーパラメータ `q=` で渡せるのですが、文字数を短くするために lz-string という Javascript のライブラリを使っておりそのエンコーディングが Python 標準では提供されていないためパラメータ文字列が生成できないためです。そのためにシンプルな URL エンコーディンでコードを渡すことができる `q2=` を新規に用意することにしました。

[https://github.com/ongaeshi/rubyonbrowser/commit/c8bc1fb9674e6cb48c2602c75f6e193b6d37f504:embed:cite]

これで準備は完了です。

# プロンプト
色々調整して ChatGPT に渡すプロンプトはこんな感じになりました。◯◯◯の箇所を生成して欲しいスクリプト種類に変更して使います。

``` 
◯◯◯ Rubyスクリプトを作成してください。
このRubyスクリプトをrubyonbrowser上で実行させたいです。

- URL は https://rubyonbrowser.ongaeshi.me/
- 後ろにクエリ文字列 ?q2=
- コードテキストを URLエンコーディングした文字列を q2 のパラメータに設定

コードと生成したURLへのリンクの順に一回で出力してください。
``` 

# 実行例1: 文字列を大文字に変換する
#### <span style="color: #5ed361">あなた</span>
文字列を大文字に変換するRubyスクリプトを作成してください。
このRubyスクリプトをrubyonbrowser上で実行させたいです。

- URL は https://rubyonbrowser.ongaeshi.me/
- 後ろにクエリ文字列 ?q2=
- コードテキストを URLエンコーディングした文字列を q2 のパラメータに設定

コードと生成したURLへのリンクの順に一回で出力してください。

#### <span style="color: #673ab7">ChatGPT</span>
以下に、文字列を大文字に変換するRubyスクリプトと、それを実行するためのrubyonbrowserへのリンクを提供します。

Rubyスクリプト:
```ruby
puts gets.chomp.upcase
```

生成したURL:
[https://rubyonbrowser.ongaeshi.me/?q2=puts+gets.chomp.upcase](https://rubyonbrowser.ongaeshi.me/?q2=puts+gets.chomp.upcase)

## 感想
実際の実行はモバイル上から行っており、リンクを生成することで特にモバイルからだと確認しやすくなります。Ruby on Browser で実行することで、コードをちょっと修正できるのも嬉しいですね。

<iframe src="https://www.youtube.com/embed/4s2NRcXuQhY?enablejsapi=1" width="560" height="315" frameborder="0" allowfullscreen></iframe><br><a href="https://www.youtube.com/watch?v=4s2NRcXuQhY">www.youtube.com</a>

# 実行例2: Hash#dig のサンプルコードを出力させる
#### <span style="color: #5ed361">あなた</span>
Hash#dig の使い方を初心者プログラマに説明するRubyスクリプトを作成してください。
このRubyスクリプトをrubyonbrowser上で実行させたいです。

- URL は https://rubyonbrowser.ongaeshi.me/
- 後ろにクエリ文字列 ?q2=
- コードテキストを URLエンコーディングした文字列を q2 のパラメータに設定

コードと生成したURLへのリンクの順に一回で出力してください。
#### <span style="color: #673ab7">ChatGPT</span>
Rubyの`Hash#dig`メソッドは、ネストされたハッシュから安全に値を取得するために使用されます。例えば、あるハッシュが複数の階層を持っている場合、`dig`メソッドを使って特定のキーの経路に沿って値を取得できます。もし指定したキーのいずれかが存在しない場合、`nil`が返されます。

初心者の方に向けた説明用のRubyスクリプトを以下に示します。このスクリプトでは、簡単なハッシュを作成し、`dig`メソッドを使用して値を取得します。

```ruby
# ネストされたハッシュの例
user_data = {
  name: "Alice",
  contact: {
    email: "alice@example.com",
    phone: {
      home: "123-456-7890",
      mobile: "987-654-3210"
    }
  }
}

# 'dig' メソッドを使用して、モバイル電話番号を取得
mobile_phone = user_data.dig(:contact, :phone, :mobile)

# 結果を出力
puts "Mobile phone number: #{mobile_phone}"
```

このスクリプトをRubyOnBrowserで実行するためのURLは以下の通りです。

[実行用URL](https://rubyonbrowser.ongaeshi.me/?q2=%23+%E3%83%8D%E3%82%B9%E3%83%88%E3%81%95%E3%82%8C%E3%81%9F%E3%83%8F%E3%83%83%E3%82%B7%E3%83%A5%E3%81%AE%E4%BE%8B%0Auser_data+%3D+%7B%0A++name%3A+%22Alice%22%2C%0A++contact%3A+%7B%0A++++email%3A+%22alice%40example.com%22%2C%0A++++phone%3A+%7B%0A++++++home%3A+%22123-456-7890%22%2C%0A++++++mobile%3A+%22987-654-3210%22%0A++++%7D%0A++%7D%0A%7D%0A%0A%23+%27dig%27+%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6%E3%80%81%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E9%9B%BB%E8%A9%B1%E7%95%AA%E5%8F%B7%E3%82%92%E5%8F%96%E5%BE%97%0Amobile_phone+%3D+user_data.dig%28%3Acontact%2C+%3Aphone%2C+%3Amobile%29%0A%0A%23+%E7%B5%90%E6%9E%9C%E3%82%92%E5%87%BA%E5%8A%9B%0Aputs+%22Mobile+phone+number%3A+%23%7Bmobile_phone%7D%22)
## 感想
実行すると "987-654-3210" が返ってきました。引数を `(:contact, :phone, :home)` に変更すると "123-456-7890" になりました。途中のパラメータ名が間違っていても例外にならず nil で返ってくるのが便利なのだと理解できました。サンプルデータも含めてコードを作成してくれるのがありがたいです。

# 実行例3: 未知のアルゴリズムを教えてもらう
#### <span style="color: #5ed361">あなた</span>
レーベンシュタイン距離を求めるRubyスクリプトを作成してください。
このRubyスクリプトをrubyonbrowser上で実行させたいです。

- URL は https://rubyonbrowser.ongaeshi.me/
- 後ろにクエリ文字列 ?q2=
- コードテキストを URLエンコーディングした文字列を q2 のパラメータに設定

コードと生成したURLへのリンクの順に一回で出力してください。

#### <span style="color: #673ab7">ChatGPT</span>
レーベンシュタイン距離を計算するためのRubyスクリプトと、そのスクリプトをRuby on Browserで実行するためのURLを生成しました。

Rubyスクリプトは以下の通りです。

```ruby
def levenshtein_distance(s, t)
  m = s.length
  n = t.length
  return m if n == 0
  return n if m == 0
  d = Array.new(m+1) {Array.new(n+1)}

  (0..m).each {|i| d[i][0] = i}
  (0..n).each {|j| d[0][j] = j}

  (1..n).each do |j|
    (1..m).each do |i|
      cost = (s[i-1] == t[j-1]) ? 0 : 1
      d[i][j] = [d[i-1][j]+1, d[i][j-1]+1, d[i-1][j-1] + cost].min
    end
  end
  d[m][n]
end

# 使用例
puts levenshtein_distance("kitten", "sitting")
```

このスクリプトをRuby on Browserで実行するためのURLは以下のリンクからアクセスできます：[Ruby on Browserでスクリプトを実行する](https://rubyonbrowser.ongaeshi.me/?q2=%0Adef+levenshtein_distance%28s%2C+t%29%0A++m+%3D+s.length%0A++n+%3D+t.length%0A++return+m+if+n+%3D%3D+0%0A++return+n+if+m+%3D%3D+0%0A++d+%3D+Array.new%28m%2B1%29+%7BArray.new%28n%2B1%29%7D%0A%0A++%280..m%29.each+%7B%7Ci%7C+d%5Bi%5D%5B0%5D+%3D+i%7D%0A++%280..n%29.each+%7B%7Cj%7C+d%5B0%5D%5Bj%5D+%3D+j%7D%0A%0A++%281..n%29.each+do+%7Cj%7C%0A++++%281..m%29.each+do+%7Ci%7C%0A++++++cost+%3D+%28s%5Bi-1%5D+%3D%3D+t%5Bj-1%5D%29+%3F+0+%3A+1%0A++++++d%5Bi%5D%5Bj%5D+%3D+%5Bd%5Bi-1%5D%5Bj%5D%2B1%2C+d%5Bi%5D%5Bj-1%5D%2B1%2C+d%5Bi-1%5D%5Bj-1%5D+%2B+cost%5D.min%0A++++end%0A++end%0A++d%5Bm%5D%5Bn%5D%0Aend%0A%0A%23+Example+usage%0Aputs+levenshtein_distance%28%22kitten%22%2C+%22sitting%22%29%0A).

## 感想
git のコマンドのスペルミスがあったときに近いコマンドを提示してくれるアルゴリズムが確かこんな名前だったという記憶だけで聞いてみました。ハルシネーションが怖いですが試した感じ正しく動いていそうな感じでした。実際に使いたいときはこれをベースに挙動を理解するとこから始めるもよさそうです。(そして調べたら git ではそれよりも少し複雑な[ダメラウ・レーベンシュタイン距離](https://ja.m.wikipedia.org/wiki/%E3%83%80%E3%83%A1%E3%83%A9%E3%82%A6%E3%83%BB%E3%83%AC%E3%83%BC%E3%83%99%E3%83%B3%E3%82%B7%E3%83%A5%E3%82%BF%E3%82%A4%E3%83%B3%E8%B7%9D%E9%9B%A) を使っているそう) 
# 実行例4: ゲームを作ってもらう
#### <span style="color: #5ed361">あなた</span>
シンプルなゲームができるRubyスクリプトを作成してください。
このRubyスクリプトをrubyonbrowser上で実行させたいです。

- URL は https://rubyonbrowser.ongaeshi.me/
- 後ろにクエリ文字列 ?q2=
- コードテキストを URLエンコーディングした文字列を q のパラメータに設定

#### <span style="color: #673ab7">ChatGPT</span>
わかりました。シンプルなRubyスクリプトを作成して、それをrubyonbrowser上で実行するためのURLを生成します。ゲームの内容について、どのようなものを希望されますか？例えば、「数当てゲーム」や「じゃんけんゲーム」など、基本的なものであれば実装可能です。

#### <span style="color: #5ed361">あなた</span>
数当てゲーム

#### <span style="color: #673ab7">ChatGPT</span>
数当てゲームのRubyスクリプトを作成します。このゲームでは、コンピュータが1から100の間でランダムに数を選び、ユーザーがその数を当てるまで推測を続けます。ユーザーが数を入力すると、コンピュータはその数が正解より高いか低いかを教えます。正解を当てるとゲームが終了します。

数当てゲームのRubyスクリプトをURLエンコーディングしました。以下のURLを使用してrubyonbrowserでゲームを実行できます。

[https://rubyonbrowser.ongaeshi.me/?q2=](https://rubyonbrowser.ongaeshi.me/?q2=%0Aputs%20%22%E6%95%B0%E5%BD%93%E3%81%A6%E3%82%B2%E3%83%BC%E3%83%A0%E3%81%B8%E3%82%88%E3%81%86%E3%81%93%E3%81%9D%EF%BC%811%E3%81%8B%E3%82%89100%E3%81%AE%E9%96%93%E3%81%A7%E6%95%B0%E3%82%92%E5%BD%93%E3%81%A6%E3%81%A6%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84%E3%80%82%22%0A%0A%23%20Generate%20a%20random%20number%20between%201%20and%20100%0Asecret_number%20%3D%20rand%281..100%29%0Aguess%20%3D%20nil%0A%0Auntil%20guess%20%3D%3D%20secret_number%0A%20%20puts%20%22%E6%95%B0%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84%EF%BC%9A%22%0A%20%20guess%20%3D%20gets.chomp.to_i%0A%0A%20%20if%20guess%20%3C%20secret_number%0A%20%20%20%20puts%20%22%E3%82%82%E3%81%A3%E3%81%A8%E5%A4%A7%E3%81%8D%E3%81%84%E6%95%B0%E3%81%A7%E3%81%99%E3%80%82%22%0A%20%20elsif%20guess%20%3E%20secret_number%0A%20%20%20%20puts%20%22%E3%82%82%E3%81%A3%E3%81%A8%E5%B0%8F%E3%81%95%E3%81%84%E6%95%B0%E3%81%A7%E3%81%99%E3%80%82%22%0A%20%20else%0A%20%20%20%20puts%20%22%E6%AD%A3%E8%A7%A3%E3%81%A7%E3%81%99%EF%BC%81%E6%95%B0%E3%81%AF%20%23%7Bsecret_number%7D%20%E3%81%A7%E3%81%97%E3%81%9F%E3%80%82%22%0A%20%20end%0Aend%0A)

リンクをクリックすると、ゲームが開始されます。楽しんでください！

#### <span style="color: #5ed361">あなた</span>
gets が使えないのでコンピュータが自動で入力するような方式にできますか？

#### <span style="color: #673ab7">ChatGPT</span>
了解しました。`gets`を使用しないで、コンピュータが自動で数を推測する方式に変更します。この場合、コンピュータは一定のルールに基づいて数を推測し、それに応じて次の推測を行います。

コンピュータが自動で数を推測する方式の数当てゲームのRubyスクリプトをURLエンコーディングしました。以下のURLを使用してrubyonbrowserでゲームを実行できます。

[https://rubyonbrowser.ongaeshi.me/?q2=](https://rubyonbrowser.ongaeshi.me/?q2=%0Aputs%20%22%E6%95%B0%E5%BD%93%E3%81%A6%E3%82%B2%E3%83%BC%E3%83%A0%E3%81%B8%E3%82%88%E3%81%86%E3%81%93%E3%81%9D%EF%BC%81%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E3%81%8C1%E3%81%8B%E3%82%89100%E3%81%AE%E9%96%93%E3%81%A7%E6%95%B0%E3%82%92%E5%BD%93%E3%81%A6%E3%81%BE%E3%81%99%E3%80%82%22%0A%0A%23%20Generate%20a%20random%20number%20between%201%20and%20100%0Asecret_number%20%3D%20rand%281..100%29%0Alow%20%3D%201%0Ahigh%20%3D%20100%0Aguess%20%3D%20nil%0A%0Auntil%20guess%20%3D%3D%20secret_number%0A%20%20guess%20%3D%20%28low%20%2B%20high%29%20/%202%0A%20%20puts%20%22%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E3%81%AE%E6%8E%A8%E6%B8%AC%EF%BC%9A%20%23%7Bguess%7D%22%0A%0A%20%20if%20guess%20%3C%20secret_number%0A%20%20%20%20puts%20%22%E3%82%82%E3%81%A3%E3%81%A8%E5%A4%A7%E3%81%8D%E3%81%84%E6%95%B0%E3%81%A7%E3%81%99%E3%80%82%22%0A%20%20%20%20low%20%3D%20guess%20%2B%201%0A%20%20elsif%20guess%20%3E%20secret_number%0A%20%20%20%20puts%20%22%E3%82%82%E3%81%A3%E3%81%A8%E5%B0%8F%E3%81%95%E3%81%84%E6%95%B0%E3%81%A7%E3%81%99%E3%80%82%22%0A%20%20%20%20high%20%3D%20guess%20-%201%0A%20%20else%0A%20%20%20%20puts%20%22%E6%AD%A3%E8%A7%A3%E3%81%A7%E3%81%99%EF%BC%81%E6%95%B0%E3%81%AF%20%23%7Bsecret_number%7D%20%E3%81%A7%E3%81%97%E3%81%9F%E3%80%82%22%0A%20%20end%0Aend%0A)

リンクをクリックすると、ゲームが開始されます。楽しんでください！

## 感想
最後は簡単なゲームを作ってもらいました。答える側の AI 部分もこちらが要求すれば作ってくれるのはすごいですね。
# まとめ
プロンプト側に URL も含めて生成させておくと後からチャットログを再確認するときや、今回のようにブログに貼り付けるときにすぐに確認できるので結構便利です。その場でコードを変更して確認できるのもよいです。是非みなさんも色々な Ruby コードを生成させて実行してみてください。

[blog:g:11696248318754550880:banner]
