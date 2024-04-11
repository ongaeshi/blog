---
Title: iOSでRubyプログラミングできるPictRuby 0.2 - アプリのランチャーになったりWebAPIを叩けるようになった
Category:
- ruby
- mruby
- rubypico
- ios
- programming
Date: 2015-12-24T23:33:07+09:00
URL: https://ongaeshi.hatenablog.com/entry/pictruby-0.2
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347149575166
---

この記事は[mruby Advent Calendar 2015](http://qiita.com/advent-calendar/2015/mruby)の25日目の記事です。前日は[mrubyのRedisクライアントのPipelining対応とDisqueクライアント - 人間とウェブの未来](http://hb.matsumoto-r.jp/entry/2015/12/24/124437)でした！

PictRuby 0.2 をリリースしました。iPhoneやiPad上でコードを編集、実行、デバッグしながらプログラムを書くことができます。

[PictRuby - Ruby Programming Environment in iOS](http://pictruby.ongaeshi.me/)

<a href="https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=1042498865&amp;mt=8" target="itunes_store" style="display:inline-block;overflow:hidden;background:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.png) no-repeat;width:135px;height:40px;@media only screen{background-image:url(https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg);}"></a></p>

## 文字列を返すとテキストファイルとして表示、コピペできるようになった

[いままで](http://ongaeshi.hatenablog.com/entry/release-pictruby)は画像オブジェクトを返すと画像を表示、保存出来ましたが文字列も返せるようになりました。

```ruby
def convert
  "Hello!"
end
```

[f:id:tuto0621:20151224232906p:plain]

文字列を返すとテキストファイルとして結果が表示されます。右上のセーブボタンを押すとクリップボードにコピーすることができます。

## 文字列にURLを含めるとタップしてブラウザを開ける

文字列内のURLは結果画面でタップできます。

```ruby
def convert
  "Hello, PictRuby\nhttp://pictruby.ongaeshi.me"
end
```

[f:id:tuto0621:20151224233027p:plain]

青文字の部分をタップするとブラウザが開きます。

[f:id:tuto0621:20151224233041p:plain]

## ポップアップメッセージを表示したり、ポップアップ経由で入力できるようになった

```ruby
def convert
  Popup.msg("Hi!")
end
```

[f:id:tuto0621:20151224233052p:plain]

```ruby
def convert
  name = Popup.input "What your name?"
  "Hello! #{name}."
end
```

[f:id:tuto0621:20151224233106p:plain]

## クリップボードにコピー

クリップボードの各行に対してヘッダを挿入します。`Clipboard.set`を使うとコード内で結果をクリップボードにコピーすることもできます。

```ruby
# Please return the text or image in the "def convert"

def convert
  src = Clipboard.get
  header = Popup.input "Header?"
  src.split("\n").map { |e| header + e }.join("\n")
end
```

[f:id:tuto0621:20151224233122p:plain]

## URL Schemeを使うとアプリを起動

入力されたキーワードで辞書アプリやサイトを串刺し検索する例です。

```ruby
# Please return the Image object in the "def convert"

def convert
  text = Popup.input("word?")
  text = URI.encode_www_form_component(text)
  
  <<EOS
  ウィズダム
  mkwisdom://jp.monokakido.mkwisdom/search?text=#{text}
  
  大辞林
  mkdaijirin://jp.monokakido.daijirin/search?text=#{text}
  
  Weblio
  http://ejje.weblio.jp/content/#{text}
  
  Wikipedia(ja)
  #{wikipedia(text, "ja")}
  
  Wikipedia(en)
  #{wikipedia(text)}
  
  Google翻訳
  http://translate.google.co.jp/?hl=ja#ja/en/#{text}
EOS
end

def wikipedia(text, country = 'en')
  "http://#{country}.wikipedia.org/wiki/#{text}"
end
```

[f:id:tuto0621:20151224233134p:plain]

[f:id:tuto0621:20151224233143p:plain]

[f:id:tuto0621:20151224233155p:plain]

## Browser.open, get, json でWebAPIを叩けるようになった

Weather Hacks から今日の天気情報を取得して表示します。

```ruby
# # ja/天気
#
# ## 概要
# Weather Hacks から今日の天気情報を取得して表示します。
#
# ## 使い方
# 1. CITYに天気を表示したい地域コードを空白区切りで追加
# 2. 地域コードは那覇だと http://weather.livedoor.com/area/forecast/471010 の最後の数字です
# 3. 実行結果をテキストにコピーすると日々の天気や気温の移り変わりを記録することもできます

CITY = %w(016010 130010 270000 471010)

def convert
  t = Time.now
  date = t.datestr
  
  "#{date} #{t.hour}:#{t.min} の天気\n\n" + 
  CITY.map { |e|
    LivedoorWeather.new(e).show(date)
  }.join("---\n")
end

class LivedoorWeather
  attr_reader :data
  
  def initialize(city)
    @city = city
    @data = Browser.json("http://weather.livedoor.com/forecast/webservice/json/v1?city=#{city}")
  end
  
  def show(today)
  <<EOS 
#{day_str(today)}の#{area}は#{telop(0)}(気温は#{temperature(0, "min")}から#{temperature(0, "max")}度)。
明日は#{telop(1)}(気温は#{temperature(1, "min")}から#{temperature(1, "max")}度)の予定。
#{url}
EOS
  end
  
  def day_str(today)
    if today == date(0)
      "今日" 
    else
      date(0)
    end
  end

  def telop(idx)
    @data["forecasts"][idx]["telop"]
  end
  
  def temperature(idx, kind)
     t = @data["forecasts"][idx]["temperature"][kind]
     t ? t["celsius"]  : "?"
  end
   
  def date(idx)
     @data["forecasts"][idx]["date"]
  end

  def area
    @data["location"]["city"]
  end
  
  def description
    @data["description"]["text"]
  end
  
  def url
    "http://weather.livedoor.com/lite/area/forecast/#{@city}"
  end
end

class Time
  def datestr
    sprintf("%04d-%02d-%02d", year, month, day)
  end
end
```

[f:id:tuto0621:20151224233207p:plain]

## 他にもいろいろできるよ
アプリ内のサンプルタブには実行可能なサンプルコードが収録されているで参考にしてください。

[PictRubyGems](https://github.com/ongaeshi/PictRubyGems) にも色々置いてあります。

## まとめ
自分でも毎日使うプログラムが書けるようになってきて満足しています(すっかりPictじゃ無くなってきましたが)。

モバイルコンピュータでプログラミングを楽しみましょう。

## 過去の記事
- [iPhoneでRubyを使って写真フィルターが書けるPictRubyをリリースしました - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/release-pictruby)
