---
Title: FireLinkがFirefox40で動かない問題を修正した
Category:
- firefox
- firelink
Date: 2015-08-15T14:54:39+09:00
URL: https://ongaeshi.hatenablog.com/entry/fix-firelink-for-firefox40
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450105677644
---

Firefox40で[FireLink](http://firelink.ongaeshi.me/)が動かなくなるという報告を何件かいただいた。(なかなか対処する時間を取れずにすいません)

Firefox40がリリースされてしまったのでこれはいよいよ直さなければなー、と思っていたら[いつも素晴らしいプルリクエスト](http://ongaeshi.hatenablog.com/entry/firelink-pullreq)を送ってくれる台湾の[chihchun](https://github.com/ongaeshi/firelink/pull/5)氏が直してくれた！さっそく取り込もう。

## 2.3.1 を申請
[FireLink - Copy link with keyboard shortcuts :: Versions :: Add-ons for Firefox](https://addons.mozilla.org/en-US/firefox/addon/firelink/versions/?page=1#version-2.3.1)

という訳で2.3.1をフルレビューで申請(ただいまレビュー待ち)。通過すれば通常のFirefox40でもFireLinkが使えるようになります。

## 署名のないアドオンをインストールするには
残念ながら最新のFirefoxでは署名のない(野良の)アドオンをインストールする方法がなくなってしまった。NightlyやDeveloper Editionなら`about:config`から`xpinstall.signatures.required = false`とすることでインストールすることができる。

Firefox40なら通常版でも`xpinstall.signatures.required`がデフォルトfalseで設定されているようです。(Thanks @toujitwtt 氏) ※ ただしFirefox41で削除される予定

- [Firefox 40 アドオン互換性情報 | Mozilla Developer Street (modest)](https://dev.mozilla.jp/2015/06/firefox-40-addon-compatibility/)
- [Fix for installing unsigned add-ons in Firefox Dev and Nightly](http://www.ghacks.net/2015/08/04/fix-for-installing-unsigned-add-ons-in-firefox-dev-and-nightly/)

## レビューが通過するまでご迷惑をおかけします
レビューが通過すれば通常のFirefoxでも使えるようになります。それまではFirefox39で待っていただくか、`xpinstall.signatures.required`をfalseにして(レビュー前の)2.3.1を使ってください。🙇










