---
Title: RubyPico 0.9.4 リリース - GitHubに置かれたスクリプトをダウンロードできるように
Category:
- rubypico
Date: 2017-01-13T00:55:51+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-0.9.4
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687206195221
---

拡張スクリプトを追加することでGitHubに置いたスクリプトをダウンロードできるようになりました。 - [更新履歴](https://github.com/ongaeshi/RubyPico/blob/master/HISTORY.md)

[https://itunes.apple.com/jp/app/rubypico/id1042498865?mt=8&uo=4:embed]

## github_download.rb
[RubyPicoGems](https://github.com/ongaeshi/RubyPicoGems)の仕組みをリニューアルしました。

1. github_download.rb をインストール
2. インストールしたいライブラリのパスをコピー(例. `ongaeshi/rubypico_github`)
3. github_download.rb を使ってライブラリをインストール

全ての手順がRubyPico内で完結するようになりました。今までとは雲泥の使いやすさなので是非お試しください。自分が書いたプログラムもGitHubに置くことで他の人に配布できるようになります。

※ とりあえず`ongaeshi/app_installer`をインストールしておくとAppタブへの登録が簡単になるのでおすすめです

## Image.load()でファイルへの読み書きが可能に
`Image.load`でローカルファイル内の画像を読み込んだり、`Image#save_to`でカメラロールの画像をファイルに保存することが可能になりました。

ということでついに、GitHubレポジトリに画像を入れてコミット、github_download.rbでダウンロード、Image.loadで表示することが可能になります。

## ホームページリニューアル
情報が複数のページに拡散していたので1ページにまとめなおしました。スマホからでも読みやすく検索しやすくなったと思います。

http://rubypico.ongaeshi.me/

## インストール
[App Store](https://itunes.apple.com/jp/app/rubypico/id1042498865)からどうぞ。
