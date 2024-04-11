---
Title: ブラウザから電子書籍の追加・削除ができるようになりました - Honyomi 1.5 リリース
Category:
- honyomi
- groonga
- ruby
- book
Date: 2015-11-23T11:51:12+09:00
URL: https://ongaeshi.hatenablog.com/entry/honyomi-1.5
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6653586347146187923
---

Honyomi 1.5 をリリースしました。

- Webインターフェースから書籍の削除、ページ画像の追加・削除ができるように
- 検索結果がサムネイル画像になって見やすく
- 書籍の解析処理の改善

詳しくは[更新履歴](https://github.com/ongaeshi/honyomi/blob/master/HISTORY.md#15---2015-11-21)をどうぞ。

## インストール
[Honyomi - インストール](http://honyomi.nagoya/ja/install.html)

ビルド済みのDockerコンテナ、もしくはRubyGemsからのインストールすることができます。
Dockerコンテナは[groonga/rroonga](https://hub.docker.com/r/groonga/rroonga/)をベースにして作るように変更しました。

## Webインターフェースから書籍の削除、ページ画像の追加・削除ができるように
[f:id:tuto0621:20151123114236j:plain]

Webインターフェースからできることが増えました。

- 書籍の追加 (今でもできる)
- 閲覧、検索 (今でもできる)
- 書籍の削除 (1.5から)
- ページ画像の追加・削除 (1.5から)

電子書籍検索エンジンに最低限必要なことがWebからできるようになりました。
デプロイすればブラウザから一通りの操作できます。

## 検索結果がサムネイル画像になって見やすく
[f:id:tuto0621:20151123114246j:plain]

検索結果画面では画像を小さめに表示してたくさんの検索結果を表示できるようにしました。

## 書籍の解析処理の改善
[WEB+DB PRESS Vol.89](http://gihyo.jp/magazine/wdpress/archive/2015/vol89)の解析処理に一部失敗している箇所があったのを修正しました。

