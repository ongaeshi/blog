---
Title: DockerをOSXやWindowsで使う時の注意点
Date: 2014-10-16T02:23:19+09:00
URL: https://ongaeshi.hatenablog.com/entry/use-docker-on-osx-and-windows
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450068616483
---

Dockerの基本的な使い方については[「いまさら聞けないDocker入門」](http://www.atmarkit.co.jp/ait/kw/docker_nyumon.html)がいいと思います。

DockerをOSXやWindowsで使う場合、加えて`boot2docker`というソフトウェアを間に挟む必要があってもう少し複雑なのでメモ。

## boot2docker?
OSXやWindowsだとDockerは動かない(Linuxでしか動かない)ので、VirtualBox上に小さなLinux(Tiny Core Linux)を動かしてその上にDockerを載せたものを<b>コマンドライン上ではOSXで動いてるように見せかける</b>ものです。

実際はVirtualBox上で動いているLinuxと通信しているだけなので、特殊な環境変数の設定が必要だったり、WebアプリをOSXのブラウザで動かす時に少し特殊な設定が必要になったりします。

[Boot2docker by boot2docker](http://boot2docker.io/)

## インストール
- [Releases · boot2docker/osx-installer](https://github.com/boot2docker/osx-installer/releases)から最新版をダウンロード
- [Installing Docker on Mac OS X](http://docs.docker.com/installation/mac/)を参考にインストール

## 起動
使う時は`boot2docekr start`します。

```shell
$ boot2docker start
Waiting for VM and Docker daemon to start...
..
Started.

To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://xx.xx.xx.xx:xxxx  # <- 注目
```

`export DOCKER_HOST=.....`をシェルに環境変数を設定します。使う度に設定する必要があります。
環境変数が設定されていないとdockerコマンドが実行出来ないので注意が必要です。

```shell
# DOCKER_HOST環境変数が設定されていない・・
$ docker images 
2014/10/15 00:24:05 Get http:///var/run/docker.sock/v1.14/images/json: dial unix /var/run/docker.sock: no such file or directory
```

環境変数を設定すると

```shell
$ export DOCKER_HOST=tcp://xx.xx.xx.xx:xxxx
```

無事動くようになります。

```shell
$ docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ongaeshi/milkode-gem          latest              47ebab29ce3e        32 hours ago        359.8 MB
<none>                        <none>              294dc46ed42a        32 hours ago        327.1 MB
<none>                        <none>              28b53e9e9b49        32 hours ago        327.1 MB
<none>                        <none>              400f06f6451a        32 hours ago        327.1 MB
```

## Dockerで立ち上げたWebアプリのコンテナをOSX上のブラウザ上で操作する
例えば [Dockerizing a Node.js web application - Docker Documentation](https://docs.docker.com/examples/nodejs_web_app/)に沿って実行するとWebアプリ立ち上げまでは上手くいくのですが`localhost:49160`にアクセスしてもWebアプリが表示されません。

先ほども言ったようにこのWebアプリは<b>VirtualBox上で実行されている</b>ので`boot2docker ip`で取得したIP経由でアクセスするばよいです。

```shell
$ boot2docker ip

The VM's Host only interface IP address is: 192.168.59.103
```

整理すると

```
$ open http://192.168.59.103:49160
```

でOKです。よいDocekrライフを。
