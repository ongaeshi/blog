---
Title: ' EmacsでObjective-C, Objective-C++を編集する'
Category:
- emacs
- ios
- objc
Date: 2014-08-09T22:53:53+09:00
URL: https://ongaeshi.hatenablog.com/entry/edit-objc-using-emacs
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815729938410
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140809/20140809225141.png" alt="f:id:tuto0621:20140809225141p:plain" title="f:id:tuto0621:20140809225141p:plain" class="hatena-fotolife" itemprop="image"></span></p>

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140809/20140809225200.png" alt="f:id:tuto0621:20140809225200p:plain" title="f:id:tuto0621:20140809225200p:plain" class="hatena-fotolife" itemprop="image"></span></p>

[RubyKokuban](http://ongaeshi.hatenablog.com/entry/rubykokuban-release)(<b>ofruby</b>に改名予定)のiOS版を作っています。

OSXの時はほとんどC++で書けたので気にならなかったのですが、iOS用に作る時はObjective-C(拡張子.m)やObjective-C++(拡張子.mm)の編集が必要です。

さっそくEmacsで編集出来るように設定を行いました。

## .emacs.d/init.el の設定
[参考: Emacs で iPhone アプリ開発を快適にするための設定 : 紹介マニア](http://sakito.jp/emacs/emacsobjectivec.html)

```lisp
(add-to-list 'auto-mode-alist '("\\.mm?$" . objc-mode))
(add-to-list 'magic-mode-alist '("\\(.\\|\n\\)*\n@implementation" . objc-mode))
(add-to-list 'magic-mode-alist '("\\(.\\|\n\\)*\n@interface" . objc-mode))
(add-to-list 'magic-mode-alist '("\\(.\\|\n\\)*\n@protocol" . objc-mode))
```

- Objective-Cを編集するための`objc-mode`は標準搭載
- 拡張子(.m, .mm)の時にobjc-modeで開く
- .hはc-mode, c++-modeの可能性があるため、`@implementation`, `@interface`, `@protocol` がテキスト無いに含まれているいる時のみobjc-modeで開くように

紹介マニアさんの設定そのままだけど上手くいきました。本当は`ff-find-other-file`でヘッダと実装ファイルを行き来出来るようにもしたかったのだけどそっちは何故か動かなかったのでまたそのうちに。






