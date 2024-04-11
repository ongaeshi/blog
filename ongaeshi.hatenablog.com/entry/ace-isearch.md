---
Title: ' Emacsのテキスト検索の使い勝手をあげるace-isearchとhelm-swoopが便利'
Category:
- emacs
- emacs
Date: 2014-10-10T00:44:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/ace-isearch
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450067940339
---

ace-isearchはisearchの文字入力に合わせて自動でace-jumpやhelm-swoopを起動してくれます。helm-swoopは超強力なoccurです。

詳しくは各作者さんのページをどうぞ。

- [Emacs - isearchとace-jump-modeの連携 - Qiita](http://qiita.com/ballforest/items/7c3f2e64b59d8157bc8c) - [GitHub](https://github.com/tam17aki/ace-isearch)
- [helm-swoopでEmacsを位置同期検索 - Web学び](http://fukuyama.co/?p=6578) - [GitHub](https://github.com/ShingoFukuyama/helm-swoop)

色々と試した結果として私の設定は

- isearchからの自動移行は1文字のace-jumpのみに
- isearch中にM-oでmulti-swoopに移行

```elisp
(require 'ace-isearch)
(global-ace-isearch-mode +1)
(setq ace-isearch-use-function-from-isearch nil)
(define-key isearch-mode-map (kbd "M-o") 'helm-multi-swoop-all-from-isearch)
```

になりました。便利です。
