---
Title: RubyPico開発日記1 - iOSからWebサーバーを起動する
Category:
- rubypico
Date: 2016-07-30T17:06:36+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-diary-1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687176732555
---

RubyPicoが内部で使っているmrubyを1.2に上げたので[matsumoto-r/mruby-simplehttpserver](https://github.com/matsumoto-r/mruby-simplehttpserver)をコンパイルできるようになった。以下のようなプログラムを書くとイントラネット内でwebサーバーを起動できるようになる。

```ruby
# 
# Server Configration
# 

server = SimpleHttpServer.new({

  :server_ip => "0.0.0.0",
  :port  =>  8000,
  :document_root => "./",
})


#
# HTTP Initialize Configuration Per Request
#

# You can use request parameters at http or location configration
#   r.method
#   r.schema
#   r.host
#   r.port
#   r.path
#   r.query
#   r.headers
#   r.body

server.http do |r|
  server.set_response_headers({
    "Server" => "my-mruby-simplehttpserver",
    "Date" => server.http_date,
  })
end

# 
# Location Configration
# 

# /mruby location config
server.location "/mruby" do |r|
  if r.method == "POST"
    server.response_body = "Hello mruby World. Your post is '#{r.body}'\n"
  else
    server.response_body = "Hello mruby World at '#{r.path}'\n"
  end
  server.create_response
end

server.run
```

[iOSデバイスのIPをアドレス確認](https://flytransfer.wordpress.com/2013/10/23/how-to-find-ip-for-ios-device/)して、

```
http://111.111.111.111:8000
```

をブラウザに打ち込むと他のマシンでアクセスできるようになる。(エミュレータの場合はlocalhost:8000で同一マシン上の他のブラウザからアクセス可能)

[mattn/mruby-sinatic](https://github.com/mattn/mruby-sinatic)みたいなWebアプリをさくっと書くための薄いラッパーがほしい。(mruby-uvがiOSでビルドできるならむしろmruby-sinaticも組み込んでしまうのがいいのかもしれない)
