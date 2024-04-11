---
Title: Rubyの開発環境を2021年ぽくする for Windows
Date: 2021-08-07T15:21:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/ruby-development-environment-for-windows
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/26006613794636402
---

2021年なのでこれくらいは欲しい。

- Ruby 2.7.4
- VSCode
- バイナリgemを確実にビルドできる
- コードフォーマッタ
- Lint
- デバッガ
- コード補完

それぞれは独立した機能なので全部入れなくてもいいと思います。(個人的には上から順に必須度が高い)

## Ruby 2.7.4
Ruby3自体は安定しているがgem周りの挙動が若干安定していなかったのでこちらを採用した。
(RubyInstallerも2.7系をまだおすすめしていた。)

https://rubyinstaller.org/downloads/

`rubyinstaller-devkit-2.7.4-1-x64.exe`をダウンロードしてインストール。

Rubyのインストール終了後にmsysなどもインストールしてくるか聞いてくるので基本的には全てインストール。

## バイナリgemのインストール
スタートメニューに「Start Command Prompt with Ruby」が作られるのでそこからgem installする。

[f:id:tuto0621:20210807102945p:plain]

(ビルドにかなり色々必要な) popplerもちゃんとインストールできて感動した。

## コードフォーマッタ
コードフォーマッタにはrufoを使うことにした。
rubocopと比べてかなり高速に動作するのがよい。
VSCodeで編集中は定期的に`Shift+Alt+F`する派なのですぐに整形できる方がありがたい。prettierと違ってnodeを必要としないのもよい。

```
> gem install -N rufo
```

[f:id:tuto0621:20210807103030g:plain]

※ 整形したコードは有名な[prettierのあれ](https://github.com/prettier/plugin-ruby)

## Lint
[Toolboxで調べる](https://www.ruby-toolbox.com/compare/prettier,rubocop,rubyfmt,rufo,standard?display=table&order=score)とかなり勢いのあるstandardを採用。
面倒な設定無しですぐに使えるのがありがたい。

```
> gem install -N standard
```

適当なRubyプロジェクトの下に移動して、以下のコマンドを実行すればいい感じでコードを修正してくれる。

```
$ cd /path/to/project
$ standardrb --fix 
```

手持ちのmrubyプロジェクトのコードも[問題なくコード修正](https://github.com/ongaeshi/ClipScript/commit/8126862f09b548938e62a10adb308d027b78ff88)できた。

ifのtrue節とfalse節で同じ変数に代入する場合に式の戻り値で代入するように変更してくれてなかなかすごい。(というかRubyのifが文じゃなくて式なことも知らなかった。こういうのも勝手に教えてくれるのがLintのいいところだと思う)

[f:id:tuto0621:20210807103441p:plain]

設定すればVSCodeからも問題のあるコードを教えてくれる。

[f:id:tuto0621:20210807145448p:plain]

## デバッガ
これは色々なところでおすすめされていたgemをそのまま使った。

```
> gem install -N debase ruby-debug-ide
```

[f:id:tuto0621:20210807145604g:plain]

デバッガをF5で起動するには`.vscode/launch.json`の設定が必要。(VSCodeのGUI上で自動生成してくれるし、手で書いてもよい)

[f:id:tuto0621:20210807145718p:plain]

## VSCodeの設定
ここまでインストールしたgemをVSCodeでも使えるようにする。

いわゆるRubyプラグインをインストールして、以下の設定を加える。

https://github.com/testdouble/standard/wiki/IDE:-vscode

- Language Serverを有効にする
  - ブロックのfold, unfoldなどが有効になる
- インテリセンスにrubyLocate
  - F12で定義ジャンプができるようになる
- Linterはstandard
- Formatterはrufo
- Debuggerは特に設定不要

settings.jsonは以下のような感じ。

```json
{
    "ruby.lint": {
        "standard": true,
    },
    "ruby.format": "rufo",
    "ruby.useLanguageServer": true,
    "[ruby]": {
        "editor.tabSize": 2
    },
    "ruby.languageServer": {
        "logLevel": "info"
    },
    "ruby.intellisense": "rubyLocate",
}
```

## コード補完
ここまででも十分に便利だが、さらにコード補完にも対応するならsolargraph gemを入れるとよい。

```
> gem install -N solargraph 
```

solargraphには専用のVSCodeプラグインが用意されている。"Ruby Solargraph"をインストール。

[https://github.com/castwide/vscode-solargraph]

補完だけじゃなくメソッドのドキュメントがその場で見れるのも便利。

[f:id:tuto0621:20210807150126p:plain]

## まとめ
コード整形、Lint、デバッガ、コード補完とコーディングに必要な機能が一通り使えるようになり大分便利になった。

この辺りのコーディング支援機能がgolangやRustのような後発の言語と違って標準で用意されていない(もしくは選択肢が多すぎてどれを使ってよいか分からない)ため、どのgemを入れればよいかを検証するのにそこそこ時間がかかった(探せば何かしらあるであろうことは分かっていたのだが)。

irbも最近はコード補完が強力になってかなり使いやすくなったため、例えばコード整形機能だけでも標準で用意されているとすごく印象がすごくよくなりそうだなぁと思った。

Rubyは成熟した言語なのでこの辺りの周辺機能の充実がより重要なのかもしれない(https://github.com/ruby/debug などの開発が進んでいるのはとても楽しみです)。

