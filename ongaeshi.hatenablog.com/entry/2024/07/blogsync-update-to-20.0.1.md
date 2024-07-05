---
Title: blogsync 20.0.1 への更新
Category:
- blogsync
Date: 2024-07-06T02:54:16+09:00
URL: https://ongaeshi.hatenablog.com/entry/2024/07/blogsync-update-to-20.0.1
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189119756540
---

# blogsync の置き場所を確認

PowerShell の type は Get-Command (いつも忘れる)。

```
> Get-Command blogsync
C:\app\bin\blogsync.exe
```

# ダウンロード
https://github.com/x-motemen/blogsync/releases から最新のバイナリをダウンロード。

https://github.com/x-motemen/blogsync/releases/download/v0.20.1/blogsync_v0.20.1_windows_amd64.zip

zip 展開してできたバイナリを `C:\app\bin` に放り込めば OK。(go で書かれたアプリは更新が簡単・・！)

# 0.20.1
この辺りが特に嬉しい。

- blogsync remove が使えるようになった
- custom-path パス未指定のドラフトを更新するたびに新しいファイルが降ってきてしまう問題が解決された

```
NAME:
   blogsync.exe - A new cli application

USAGE:
   blogsync.exe [global options] command [command options] [arguments...]

VERSION:
   0.20.1 (9760a4c)

COMMANDS:
   pull     Pull entries from remote
   fetch    Fetch entries from remote
   push     Push local entries to remote
   post     Post a new entry to remote
   list     List local blogs
   remove   Remove blog entries
   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   -C PATH        Run as if blogsync was started in PATH instead of the current working directory. [%BLOGSYNC_WORKDIR%]
   --help, -h     show help
   --version, -v  print the version
```
