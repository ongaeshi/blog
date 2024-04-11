---
Title: GitHubの気になるユーザーのレポジトリをiOSで一覧表示する
Category:
- rubypico
Date: 2016-09-24T14:33:25+09:00
URL: https://ongaeshi.hatenablog.com/entry/show-github-users-repos
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687186004518
---

次のバージョンの[RubyPico](http://rubypico.ongaeshi.me/) 0.9 からリンク付き文字列を生成できるようになった。いちいちURLを表示しないですむ。

```ruby
puts AttrString.new("foo", link: "http://ongaeshi.me")
```

GitHubのレポジトリ一覧を表示してみる。

```ruby
def repos(user)
  json = Browser.json("https://api.github.com/users/#{user}/repos?sort=pushed&per_page=8")

  puts "#{user}'s repos"
  json.each do |e|
    puts AttrString.new(e["name"], link:  e["html_url"])
  end
end

repos('ongaeshi')

loop do
  print "user?> "
  name = gets
  repos(name)
end
```

最初に自分のレポジトリを表示する。2回目以降はユーザー名を入力してその人の最近のレポジトリを見ることができる。

[f:id:tuto0621:20160924143357p:plain]

matzはstreemやってるとかいつでも分かるようになった。
