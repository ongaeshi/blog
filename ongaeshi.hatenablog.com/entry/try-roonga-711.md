---
Title: Rroonga 7.1.1 がリリースされたので動作確認
Category:
- 開発環境
Date: 2018-02-01T23:43:45+09:00
URL: https://ongaeshi.hatenablog.com/entry/try-roonga-711
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812342952598
---

インストール

```
$ gem install rroonga
Installing ri documentation for rroonga-7.1.1-x64-mingw32
Done installing documentation for rroonga after 6 seconds
1 gem installed
```

動作確認。`Groonga::BINDINGS_VERSION`という定数を見るのがよさそうな予感。

```
$ ruby -r rroonga -e "p Groonga::BINDINGS_VERSION"
[7, 1, 1]
```

Milkodeも一通り動く、万歳！MLに報告して終了。

## まとめ

まとめると以下の手順でMilkodeをWindows10 Ruby2.4でインストールすることができるようになる。eventmachineをあらかじめインストールしておく[経緯はこちら](http://ongaeshi.hatenablog.com/entry/milkode-windows-10-ruby2.4)。

```
$ gem install eventmachine --platform=ruby
$ gem install milkode
```

