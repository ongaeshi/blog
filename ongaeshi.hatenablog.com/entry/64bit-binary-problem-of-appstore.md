---
Title: 自作アプリがAppStoreの64bitバイナリ問題で新バージョンを申請出来なくなった
Category:
- ios
- ofruby
Date: 2015-06-03T23:52:02+09:00
URL: https://ongaeshi.hatenablog.com/entry/64bit-binary-problem-of-appstore
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450096376573
---

<span style="font-size: 150%">※ [Qiita経由](http://qiita.com/ongaeshi/items/a3ac45baa259af5740c3)で追加情報をもらいました。
</span>

[ofruby](http://ofruby.tokyo)の新バージョンを申請しようとしたらXCodeのValidateでひっかかりました。せっかく作った新機能がリリース出来なくて困っています(AppStoreのスケジュールを確認していていなかった私が悪いのだけど)

## 基本情報
- ofrubyは[openFrameworks](http://openframeworks.jp/)+[mruby](http://www.mruby.org/)で作られている
- openFrameworksにはiOSアプリを作るためのxcodeprojがあるのでそれをベースに作っていた

## 問題

1. AppStoreは今年の頭から **64bitバイナリを含まない新規アプリの登録を禁止** にした
2. 6/1から **既存アプリの更新も64bitバイナリを含めることが必須** になった
3. openFramewokrsの0.8系はiOSの64bitビルドができない
4. openFrameworks[0.9で64bitサポート](http://forum.openframeworks.cc/t/help-64bit-support-on-ios/17057)そしてC++11対応(!)されるらしいがまだリリースされていない
5. ざっとフォーラムの情報とか追ったけど[何時頃リリース予定か](https://github.com/openframeworks/openFrameworks/issues/3039)は見つけられなかった
6. ひとまずopenFrameworksが64bitビルドされるまではAppStoreには新バージョンをリリースすることは出来なさそう

しばらくopenFramworksの情報を追いかけつつ、ofruby自体にも64bit対応が必要そうなので[Cocoa Touch 64ビット移行ガイド](https://developer.apple.com/jp/documentation/CocoaTouch64BitGuide.pdf)とかにも目を通して見ようと思います。

シンタックスハイライトとかFiberとか色々いれたんですが・・残念です。XCodeお持ちの方は[ソースコード](https://github.com/ongaeshi/ofruby-ios)から野良ビルドすれば使えるようになると思います。

[f:id:tuto0621:20150603235059g:plain]

## お願い

openFrameworks 0.9 のリリース予定など、何か情報をお持ちの方がいましたら教えてもらえると助かります。


