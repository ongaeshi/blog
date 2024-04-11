---
Title: HonyomiがDocker経由で簡単にインストールできるようになりました
Category:
- ruby
- honyomi
- docker
Date: 2015-07-24T01:21:07+09:00
URL: https://ongaeshi.hatenablog.com/entry/docker-honyomi
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450102763356
---

[f:id:tuto0621:20150724012236p:plain]

[ongaeshi/honyomi Repository | Docker Hub Registry](https://registry.hub.docker.com/u/ongaeshi/honyomi/)

DockerHubにイメージを置いたのでDockerを使っている人は簡単にHonyomiが使えるようになりました。

## インストール

[Kitematic](http://qiita.com/ongaeshi/items/1e6fd25d3c9c27f6f376)を使う場合は

- `+New`をクリック
- `honyomi`で検索して、`CREATE`をクリック
- 歯車のボタンを押して`ACCESS URL`をクリック

の3STEPで完了です。Groonga, popplerのインストールも終わっているのですぐに使えます。

コマンドラインからのインストールは、

```
$ docker run --name my-honyomi -it -p 9295:9295 ongaeshi/honyomi
```

です。

## 使い方

[DockerHub](https://registry.hub.docker.com/u/ongaeshi/honyomi/)に書きました。

- port指定しての起動
- バックグラウンドで起動
- shellの起動
- honyomi gem のアップデート - データのサルベージ
- データをホストOSに置いてコンテナ起動(Linuxのみ)

などが可能になっています。

Dockerコンテナを用意するとユーザーは複雑なインストール手順を踏まずにすぐに使えるので大変便利ですね。ツール作成者側から見てもDockerfileさえ用意出来ればどんな言語で作ったアプリケーションでも気にせず使ってもらえるというメリットがあります。自分の好きな言語で好きなライブラリを使って開発出来ますね。










