---
Title: ソースコードを読んでみた - bgm.rb
Category:
- ruby
- gem
- bundler
- milkode
Date: 2015-01-30T23:14:04+09:00
URL: https://ongaeshi.hatenablog.com/entry/read-the-code-of-bgmrb
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450081889373
---

[ターミナルから簡単に曲を聞けるbgm.rbというのを作った - hitode909の日記](http://hitode909.hatenablog.com/entry/2015/01/24/173901)

[Rubyのgemのソースコードを効率的に読む方法](http://ongaeshi.hatenablog.com/entry/read-a-ruby-gem-code-efficiently)を使ってbgm.rbのコードをざっと読んでみました。bundlerは実行環境なのでちょっとずつ挙動を変えながらコードを読めるのがよいです。2015年で今更だけどbundler本当に便利だ・・。

```
bundle exec -- ruby bgm.rb "椎名林檎"
```

しながらコード読みました。

## 図
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20150130/20150130231222.png" alt="f:id:tuto0621:20150130231222p:plain" title="f:id:tuto0621:20150130231222p:plain" class="hatena-fotolife" itemprop="image"></span></p>

## bgm.rb
[bgm.rb](https://github.com/hitode909/bgm/blob/master/bgm.rb)


本体です。BGMクラスの定義が上部にBGMクラスのインスタンスを使うメイン処理が下部にあります。

BGMクラスは大きく分けて

- キーワードにマッチする楽曲データを取得
- 楽曲データから音楽ファイル(m4a)のURLを取得、ダウンロード
- ダウンロードした音楽ファイルを再生(デフォルトは`afplay`)

OSXって`afplay`っていうコマンドで音楽再生出来るのですね。

```ruby
  def play(item)
    file = download(item)

    command = "#{@player || 'afplay'} #{file.path}"

    if @rate
      command += " --rate #{ @rate } "
    end

    if async?
      command += " &"
    end

    system command
  rescue Interrupt
    nil
  end
```

楽曲データの取得は'itunes-search-api'というGemに任せているようなのでそちらも読んでみます。

```ruby
class BGM
  def each(term)
    tracks = ITunesSearchAPI.search(term: term, country: "JP", media: "music", limit: async? ? 10 : 200)
    tracks.shuffle! if @shuffle
    tracks.each{|track|
      yield lookup track
    }
  end
  .
  .
```

## itunes-search-api gem
[rlivsey/itunes-search-api](https://github.com/rlivsey/itunes-search-api)

`HTTParty`というgemを使ってAPIを定義しているようです。おそらくgetベースの特定のURLを叩くとjsonが返ってくるAPIのようです。

```ruby
require 'httparty'

class ITunesSearchAPI
  include HTTParty
  base_uri 'http://ax.itunes.apple.com/WebObjects/MZStoreServices.woa/wa'
  format :json

  class << self
    def search(query={})
      get("/wsSearch", :query => query)["results"]
    end

    def lookup(query={})
      if results = get("/wsLookup", :query => query)["results"]
        results[0]
      end
    end
  end
end
```

[README.rdoc](https://github.com/rlivsey/itunes-search-api/blob/master/README.rdoc)を読んでみるとiTunes APIのドキュメントへのリンクがありました。

```rdoc
= ITunesSearchAPI

Ruby interface to the ITunes Search API

http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html
```

## 得られた知見
- iTunesの楽曲データを取得したい時は`itunes-search-api` gemを使う
- ダウンロードした音楽データの再生にはOSXだと`afplay`
- `HTTParty`というgemを使うとAPIのラッパークラスを簡単に書ける


