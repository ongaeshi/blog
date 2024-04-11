---
Title: はてなブログをコマンドラインから投稿できるようにした
Category:
- diary
Date: 2016-08-02T00:32:09+09:00
URL: https://ongaeshi.hatenablog.com/entry/hatenablog-writer
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687177108619
---

この記事がすでにコマンドラインで投稿している。
hateblog-writeというのをインストールしてみた。

- [はてなブログライターを作った - blog.kymmt.com](http://blog.kymmt.com/entry/hatenablog-writer)

## インストール
[はてなブログ API 用の gem を書いた - blog.kymmt.com](http://blog.kymmt.com/entry/hatenablog_gem)に沿ってコンシューマキー、アクセストークンの順に取得、config.ymlを生成すればよい。

### 落とし穴1 権限
read_private, write_private はデフォルトOFFなので注意。全ての権限を有効にしておくこと。

```
$ get_access_token CONSUMER_KEY CONSUMER_SECRET
/opt/local/lib/ruby2.3/gems/2.3.0/gems/oauth-0.4.7/lib/oauth/consumer.rb:178:in `request': parameter_rejected (OAuth::Problem)
	from /opt/local/lib/ruby2.3/gems/2.3.0/gems/oauth-0.4.7/lib/oauth/consumer.rb:194:in `token_request'
	from /opt/local/lib/ruby2.3/gems/2.3.0/gems/oauth-0.4.7/lib/oauth/consumer.rb:136:in `get_request_token'
	from /opt/local/lib/ruby2.3/gems/2.3.0/gems/hatenablog-0.2.2/exe/get_access_token:26:in `get_request_token'
	from /opt/local/lib/ruby2.3/gems/2.3.0/gems/hatenablog-0.2.2/exe/get_access_token:50:in `get_access_token'
	from /opt/local/lib/ruby2.3/gems/2.3.0/gems/hatenablog-0.2.2/exe/get_access_token:60:in `<top (required)>'
	from /opt/local/bin/get_access_token:23:in `load'
	from /opt/local/bin/get_access_token:23:in `<main>'
```

### 落とし穴2 config.yml
うまくいかないときは大抵の場合config.ymlの設定をミスっている。はてなブログIDはドメインを指定する。私だったら`ongaeshi.hatenablog.com`とか。

```
consumer_key: <コンシューマキー>
consumer_secret: <コンシューマシークレット>
access_token: <アクセストークン>
access_token_secret: <アクセストークンシークレット>
user_id: <ユーザ ID>
blog_id: <はてなブログ ID>
```

```
.
.
.
blog_id: ongaeshi.hatenablog.com
```

### 落とし穴3 ロケール
ロケールを設定しておかないと日本語を含むマークダウンを投稿できないので注意。以下を`~/.profile`や`~/.bashrc`に設定。

```
export LC_ALL=ja_JP.UTF-8
export LANG=ja_JP.UTF-8
```

## 使い方

```
$ cd ~/Documents/hatenablog-writer
$ cat blog/001/md
Test

This is test
```

投稿

```
$ bundle exec hw -c diary blog/001.md
```

更新

```
$ bundle exec hw -u blog/001.md
```

## 改造

hatenablog-writer/hw に以下のコードを足すと更新後に記事のURLを出力してくれて幸せになれます。

```diff
HBWriter.new do |hb_writer|
  ARGV.each do |file_name|
    entry_text = ''
    open(file_name, 'r') do |f|
      if OPTS[:d]
        entry_text = hb_writer.delete_entry(f.read)
      elsif OPTS[:u]
-        hb_writer.update_entry(f.read, CATEGORIES)
+        e = hb_writer.update_entry(f.read, CATEGORIES)
+        puts e.uri
      elsif OPTS[:m]
        hb_writer.minor_update_entry(f.read, CATEGORIES)
      else
        entry_text = hb_writer.post_entry(f.read, CATEGORIES)
      end
    end
```

```
$ bundle exec hw -u blog/001.md
http://ongaeshi.hatenablog.com/entry/hatenablog-writer
```

## バグ報告
動かないパターンを見つけたのでIssueに登録した。

[contentに'<'が含まれるとエラー · Issue #1 · kymmt90/hatenablog-writer](https://github.com/kymmt90/hatenablog-writer/issues/1)
