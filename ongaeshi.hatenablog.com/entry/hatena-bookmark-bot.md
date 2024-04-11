---
Title: はてなブックマークのTOP5をスマホがしゃべりはじめるPictRuby botを書いた
Category:
- rubypico
Date: 2016-04-17T17:51:03+09:00
URL: https://ongaeshi.hatenablog.com/entry/hatena-bookmark-bot
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792371487718
---

[チェレンコフ](https://twitter.com/cherenkov)さんの書いた[エビ中とLINEしてる気分になれるChatBot](http://qiita.com/cherenkov/items/c6a744639a39a693810b)が面白かったので真似してはてなブックマーク版を作りました。はてなブックマークじゃなくてもRSSさえあればなんでもしゃべります。

## 使い方
`総合`とか`世の中`とか言うと各カテゴリ最新の5件を表示してくれます。

[f:id:tuto0621:20160417174451p:plain:w462]

最新の5件を表示したあと、`1`から`5`までの数字を入力すると記事の概要をしゃべり始めます。

[f:id:tuto0621:20160417174508p:plain:w462]

相槌を打つと続きをどんどんしゃべります。

[f:id:tuto0621:20160417174535p:plain:w462]

最後にそのURLが表示されるのでクリックするとそのページが表示されます。

[f:id:tuto0621:20160417174549p:plain:w462]

`bm`と入力することでそのページをはてなブックマークアプリで開いてくれます。(概要の紹介途中でもOK)

[f:id:tuto0621:20160417174600g:plain:w462]

`カテゴリ`と入力するとことで入力可能なカテゴリ一覧が表示されます。

[f:id:tuto0621:20160417174628p:plain:w462]

文字だけでサクサク見れるのが快適です。

## インストール
以下のソースコードを[PictRuby](http://pictruby.ongaeshi.me/)に貼り付けてください。[get_sample](http://ongaeshi.hatenablog.com/entry/read-pictrubygems-script-from-pictruby-app)から`ja/hatena_bookmark_bot`でもインストールできます。

```ruby
# Define "main" function or "Chat" class

class Chat
  def initialize
  end
  
  def welcome
    "カテゴリを入力 (例: 総合, 世の中)"
  end
  
  def call(input)
    url = RSS[input]
    
    if input == "カテゴリ"
      return RSS.keys.join("\n")
    end
    
    # argv = input.split(/\s+/)
    argv = input.split(" ")
    
    if argv[0] == "bm"
      url = argv[1] || @manuscript.last
      bookmark url
      return "#{url} をブックマーク"
    end

    if url
      feed = FeedReader.new url
      @items = feed.data['query']['results']['item']
      
      return sitemap
    end
    
    if (1..5).include? input.to_i
      @item = @items[input.to_i - 1]
      
      @manuscript = []
      @manuscript.push @item['title']
      
      description = @item['description']
      if description
        @manuscript.concat description.split("。").map { |e| e + "。" }
      end
      @manuscript.push @item['link']
      @i = 0
    
      txt = @manuscript[@i]
      @i += 1
      return txt
    end
    
    if @manuscript && @manuscript[@i]
      txt = @manuscript[@i]
      @i += 1
      return txt
    end
    
    "?"
  end
  
  private
  
  def sitemap
    i = 0

    @items.map do |e|
      i += 1
      "#{i} #{e['title']}"
    end.join("\n")
  end
  
  def bookmark(url)
    Browser.open "hatenabookmark:/entry?url=#{url}"
  end
end

class FeedReader
  attr_reader :data
  
  def initialize(url)
    feed = url
    count = 5
    
    url = "https://query.yahooapis.com/v1/public/yql?" + 
    URI.encode_www_form(
      q: "SELECT title,link,description,encoded FROM rss WHERE url=\"#{feed}\" | truncate(count=#{count})",
      format: "json"
      )

    @data = Browser.json url
  end
end

RSS = {
  "総合" => "http://b.hatena.ne.jp/hotentry.rss",
  "世の中" => "http://b.hatena.ne.jp/hotentry/social.rss",
  "政治と経済" => "http://b.hatena.ne.jp/hotentry/economics.rss",
  "暮らし" => "http://b.hatena.ne.jp/hotentry/life.rss",
  "学び" => "http://b.hatena.ne.jp/hotentry/knowledge.rss",
  "テクノロジー" => "http://b.hatena.ne.jp/hotentry/it.rss",
  "アニメとゲーム" => "http://b.hatena.ne.jp/hotentry/game.rss",
  "エンタメ" => "http://b.hatena.ne.jp/hotentry/entertainment.rss",
  "おもしろ" => "http://b.hatena.ne.jp/hotentry/fun.rss",
  "動画" => "http://b.hatena.ne.jp/video.rss",
}
```

## おまけ
RSSだったらなんでも渡せるのでよく見るブログも追加できます。

```ruby
RSS = {
  .
  .
  "前園" => "http://lineblog.me/maezonomasakiyo/index.rdf",
  "エビ中" => "http://lineblog.me/ebichu/index.rdf",
}
```

[f:id:tuto0621:20160417174644p:plain:w462]

## 余談

[https://twitter.com/ongaeshi/status/721600446960246786:embed#PictRubyの名前を変えるためのアンケート募集です https://t.co/3NAP2B21pi]



