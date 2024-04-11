---
Title: ' (kantou-emacs #x01) #関東Emacs にいってきました'
Category:
- emacs
- diary
Date: 2014-09-01T01:14:15+09:00
URL: https://ongaeshi.hatenablog.com/entry/kantou-emacs-01
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815731688208
---

[(kantou-emacs #x01) #関東Emacs : ATND](https://atnd.org/events/54734)

関西Emacsに行きたいけど遠方でなかなかいけなかったので関東で開催されないかなぁと待ち望んでいました。普段はネットの情報に頼ることが多いので実際に使っている人達のノウハウを見ることが出来たら刺激になりそうだと思い参加しました。(そして実際に大変刺激になりました。)

ハッシュタグ[#関東emacs](https://twitter.com/hashtag/%E9%96%A2%E6%9D%B1emacs)で検索すると会議中やその後の雑談が読めます。

## ポジションペーパー
[ポジションペーパー一覧](http://peccu.sytes.net/clack/list)

> 今回もポジションペーパーを使います．
> 
> 参加者の交流を図るために，A4で1枚程度の内容をスクリーンに映しながら，全員が自己紹介をします．

ポジションペーパーというスタイルの勉強会は初めてだったのですがとてもよかったです。全員がポジションペーパーを見ながら発表するのでみんなが参加している感じになるし通常の発表と比べて敷居が低いのもよいです。(LTよりもさらに気軽な感じ)

後で仲良くなった人のポジションペーパーを読み返したりも出来るのでなかなか素敵です。

## 発表

### 暗黒美夢王 (あんこくびむおう)

何故か参加のVimの人です(笑)。Vim目線からのEmacsの使いにくい点やコミュニティの問題などを指摘していました。

Vimでは互換性を重視しているので10年前のプラグインは結構動くというのはいいなぁと思いました。

初心者向けの情報が少ない(各OS向けのインストールの仕方や初期設定など)のも敷居を上げているのかな？と感じました。IDEはインストールしたら特に勉強しなくてもすぐに使えるので、勉強しなくてもある程度は使えるようになっているのが大切なのかもしれません。

* Emacs界隈を元気にしたい
* Vimを選んだ理由を解説
* Vimは10年前のプラグインが動作する
* Emacsは情報が分散しすぎている
* Emacsがこの先生き残るには
* しかし日本のEmacs界には元気がない
* Googleでの検索トレンド Vim:変わらず, Emacs:下がった
* IntelliJとEmacsが戦っている
* Emacsバリアントの現状
* vim-jpとEmacs JP (vim-jp活発！)
* lingr vim: 425200, emacs: 5200
* reddit vim:17923, emacs: 8058
* 愛が足りない
* ダークイーマックスマスター
* 老害扱い
* emacs-jp活用してほしいなぁ
* neovim
* ユーザーが増えたら
* 初心者向け情報がない
* jvim
* 本家に日本語化パッチ投げる人がいる
* FSFに署名して著作権を放棄しないといけない(大変)
* Emacs JPに日本語版の情報をまとめたらどうか

### 実行例を表示する拡張つくりました(@pogin503さん)

Emacsのコマンドの実行例をRっぽく表示してくれる拡張です。

[pogin503/r-like-example](https://github.com/pogin503/r-like-example)

* r-like-example.el (仮)
  * Emacs Lispの実行例を呼び出すEmacs Lisp(R風らしい)
  * いい名前が欲しい
  * 実例を表示してくれる
  * デモデータの登録も出来る
  * 拡張開発者がサンプルデータを入れられるようにして欲しい
* タブの矢印はなんなんですか tabbar-mod

### たのしいモードレス日本語入力ac-mozc (@igjitさん)
IMEをトグルせずに日本語と英語がどんどん入力されていくデモは衝撃的でした。(実際に見るとすごくインパクトが大きかったです)

[ac-mozc (kantou-emacs #x01)](http://ja.slideshare.net/igjit/ac-38506660)

* IME何使ってます？
  * ネイティブIME: たくさん
  * SKK: 4人
* 入力メソッドの操作法を覚えるのめんどくさい・・
  * なので切り替え無しで日本語も英語も入力出来るプラグイン作りました
  * バッファ中の英語も補完できる
* Mozcすごい(Goolge日本語入力のオープンソース版)
  * 辞書は内蔵なので通信不要らしい
  * mozc.el
  * ac-mozc.el
* 動作確認
  * Ubuntuならすぐ使える
  * Macでも動かしたい
  * mozc emacs helper
  * ビルドできない人多数
* そこでVagrantでMozc serverを動かす
  * [igjit/vagrant-emacs-mozc](https://github.com/igjit/vagrant-emacs-mozc)
  * ubuntuいれる
  * vagrant-emacs-mozc
  * Windowsでもいける？(何故かおそい)
  * SSHにはPutty

### MilkodeとM-x milkode:jump-from-browserの紹介 (@ongaeshi)

Milkodeとmilkode.elを紹介してきました。私がEmacs使いなので割とMilkodeとEmacsの親和性は高いです。Milkode知っている人は結構いたので励みになりました。

* Milkode
  * Ruby + Groonga
  * コマンドラインから
  * ブラウザから検索
* milkode.el
  * M-x milkode:search
* ブラウザで開いているソースコードを直接開く
  * "def test"
  * M-x milkode:jump-from-browser
  * 内訳
    * MozReplでURLを取得: `http://127.0.0.1:9292/home/milkode/lib/milkode/cdstk/cdstk.rb?query=split&shead=package#n39`
    * URLからMilkodeパスを切り出してMilkodeに実ファイル名を問い合わせ
    * Emacsで開く、指定行番号に移動

質問、要望

* データベースに追加するのにどれくらいかかる？
  * 中規模のプロジェクトなら1分もかからない印象
* Groonga使おうとして上手く行かなかった
  * Rroonga経由で使うと比較的簡単かも
* Chrome対応してほしい
  * だよねー
  * URL取りたい
  * 3/4位がChromeだった(残りはFirefoxが多数)
  * [ChromeRepl](https://github.com/kzys/chromerepl)ってのがあるらしい

### Emacs子ネタ recycle.el (@fbkanteさん)

2ストローク、3ストロークのコマンド実行の代わりに使うことが出来るrecyle.elの紹介です。最初に入力したコマンドをキーにして好きなコマンドを割り当てることが出来ます。2つあるので上下移動などに割り当てると便利そうです。

* [fbkante/recycle](https://github.com/fbkante/recycle)
* 繰り返すを工夫すると結構便利。
  * 特定コマンドを実行した後にrecycleコマンドを打つと指定したコマンドを実行してくれる
  * recycle と recycle-2nd で2つ定義出来る
  * よく使うのはカーソル移動。
  * [設定例](https://github.com/fbkante/recycle/blob/master/recycle.el#L21)

好きなキーをrecycleとrecycle-2ndに割り当てて使う。

```elisp
(global-set-key (kdb "C-.") 'recycle)
(global-set-key (kdb "C-,") 'recycle-2nd)
```

### orgモードについて (@takaxpさん)

広大なorg-modeを使いこなす@takaxpさんのorg-mode紹介。Emacsの中でTODO管理から表計算、コードブロック実行まで何でもこなしてくれます。実際に動かしている所をみるとやはり便利そうです。開発も活発でMLの投稿量もEmacs本体と同じ位の流量が流れるそうです。

私自身は一つのテキスト内で複数種類のプログラムを実行することが出来るコード内ブロック実行がお気に入りです。markdown-modeに移植出来ないかなぁ。

* プレゼン自体が org の presentaion-mode で動いていた
* Macで使うのが辛いのか？
  * NO
  * ただし、野良ビルドしています。
  * MacEmacs-JP版もよい
   * homebrewがいいよ: brew update
* C-c C-c するとチェックボックス
* BEGIN_SRC emacs-lisp
* Google地図がみれる
* cycle-buffer.el と recentf.el で横方向に移動
* 辞書が便利
  * OSXの辞書の結果を流し込む
* C-hは何に使いますか？
  * help
  * f1は上(ウィンドウ位置移動)、Ctrl-2で真ん中
* 表計算出力
* init.org -> init.el 生成
* org-enchrypt-entry すごい
* 表計算システム
* org-babel
* webサーバーにデプロイする
  * scpでコピーしてデプロイ
  * http://orzmode.org/kanto/kanto.html
* バッファ内コマンド実行便利そう
   * BEGIN CODE ブロック
   * 一つのテキスト内でemacslispとC++のコードが混在して実行出来たのにはどよめきが起きた
   * コードの色づけもちゃんと出来たように見えた、すごい
  * M-x load-library
     * ob-で入っているのが使える
* orgモードは全部を使おうとしない(膨大すぎる)
  * 最初はTODO管理などから始めるとよい

### ox-pandocについて (@kawabataさん)

Pandocをemacs上で便利に使えるox-pandocの紹介です。docx形式やepubに簡単に出力出来るのはすごかったです。(縦書きにも対応していました) 他にもuse-packageを使った依存関係ベースのinit.el管理や絵文字の表示、palletを使ったcaskを使ったpackage管理などがとても勉強になりました。

後、漢字を部首ごとにばらしたり、ある部首をもった指定画数の漢字を一瞬で探すことが出来るids-editが面白かったです。

* [Pandoc - About pandoc](http://johnmacfarlane.net/pandoc/)
* [ox-pandoc](https://github.com/kawabata/ox-pandoc)
* M-x org-pandoc-to-docx-and-open
* ソースコードハイライト
* ビブログラフィ: 引用を簡単にする仕組み
* M-x org-pandoc-to-slidy-and-open
* mathjax
* epubにも出せる
* M-x org-pandoc-to-epub3-and-open
* なんと縦書きにも
* emacs設定
  * abc順に並べる
  * 順番は依存関係を記述することで解決
  * use-packageで設定管理
* パスワード管理ツール
  * gnus/auth-source.el
* 漢字をバラバラにしたりくっつけるツール
  * 画数で調べたり出来る
  * Webで検索出来るように
  * [ids-edit](https://github.com/kawabata/ids-edit)
* homebrewの山本パッチ使うとすぐに絵文字が使える
* パッケージ管理ツール
  * paradox
  * pallet - パッケージインストールしたものをcask管理してくれる
  * cask
* `emacs --script` で使う
* cask ... `emacs --script` すればライブラリを実行出来る


## 懇親会
ビールが発泡酒じゃなくてちゃんとしたビールだったのか(YEBISU?)とても美味しかったです。これで本当は発泡酒だったらどうしよう。

Emacs界にもカリスマやスターな人がいたら若い人が集まるのではないか、という話があったので、時折Emacsのディープな話が聞けるPodcastの[Rebuild.fm](http://rebuild.fm/)を紹介してみました。

Emacsウィンドウの不透明と半透明とコマンドで切り替えられるようにすると便利(集中する時は不透明にして、後ろのブラウザなどを見たい時は半透明にする)という話はかなりテンションが上がったので後でやってみようと思います。

## 感想
色々と情報収集出来たので、これから少しずつ自分のEmacsをパワーアップさせていこうと思います。

* cycle-buffer.el
* org-modeのコードブロック実行環境を整える
* ウィンドウの不透明と半透明とコマンドで切り替えられるようにする
* [smart-newline.el](https://github.com/ainame/smart-newline.el)
* r-like-example.el
* ox-pandoc (ポジションペーパーとかスライドがEmacsだけで簡単に作れる)
* recycle.el
