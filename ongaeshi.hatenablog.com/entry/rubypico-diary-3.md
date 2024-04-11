---
Title: RubyPico開発日誌3 - getsを実装する
Category:
- rubypico
Date: 2016-08-19T02:19:35+09:00
URL: https://ongaeshi.hatenablog.com/entry/rubypico-diary-3
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328749687179784027
---

Popup.input()を使えば今でもプログラムに入力を渡すことはできるのだが、ポップアップ中は後ろの画面にアクセスできなくなってしまうのが不便。という訳でgetsを実装したい。

```ruby
puts "Please input >>>"

while l = gets
  break if l.empty?
  puts l
end

puts "The end."
```

[iConsole](https://github.com/nicklockwood/iConsole)がそれっぽい画面だったので参考に軽く実装。それっぽい画面にはなった。

[f:id:tuto0621:20160819021650p:plain]

しかしいくつかの不満が残る。(上から致命度が高い)

- プログラム起動直後に`[_inputField becomeFirstResponder]`すると入力がソフトウェアキーボードの下に隠れてしまう
- LINEのように複数行入力できるようにしたい
- getsを使用しないときは入力部分は非表示にしたい(OR とても小さくしたい)、代案としては入力不可

いくつかモンキーパッチを試してみたがうまくいかない。この辺りでiOSのUI周りのプログラムに対するレベルアップが必要な気がしてきた。

cocoapodsで参考になりそうなpodsを探すと以下が良さそう。

- [slackhq/SlackTextViewController: A drop-in UIViewController subclass with a growing text input view and other useful messaging features](https://github.com/slackhq/SlackTextViewController)
- [SlackTextViewControllerを読んだ - AnyType](http://naoty.hatenablog.com/entry/2014/11/18/212355) - 日本語の解説記事

知りたいことは以下。

- 非storyboardでAutoLayoutを使ったUIの構築
- プログラム起動直後にフォーカスを移してもUIがソフトウェアキーボードの下に隠れないように
- 複数行の入力

その他、今は使わないけど面白そうな機能。

- Autocompletion (絵文字の補完)
- Markdonw Formatting
- Typing Indicator (プログラムからの入力補助に使えるかも？)
- Panning Gesture
- Shake Gesture
- External Keyboard (キーボードショートカット)

しばらく時間をかけてコードを読んでみよう。まずは動かすところから。
