---
Title: A Tour of Go を一通りやってみて、 Go言語の印象に残った機能や特徴などの感想
Category:
- go
- emacs
Date: 2014-05-05T11:38:11+09:00
URL: https://ongaeshi.hatenablog.com/entry/go-language-impressions
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815723286660
---

[はじめてのGo言語 - ブログのおんがえし](http://ongaeshi.hatenablog.com/entry/_Go-language-for-the-first-time) に続いて [A Tour of Go](http://tour.golang.org/) を最後までやってみました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140505/20140505113756.png" alt="f:id:tuto0621:20140505113756p:plain" title="f:id:tuto0621:20140505113756p:plain" class="hatena-fotolife" itemprop="image"></span></p>

コードを実際に実行させながら基本的なGoの使い方を学ぶことで出来るのでおすすめです。日本語版もあります。

英語の勉強にもなるので私は英語版を使いました。どうしても意味が分からない時だけ日本語版を読むようにしたのですが、コードもあるし全体の80%くらいは英語だけでなんとか分かりました。プログラマはコード例などが付いているものの方が英語学習が捗ると思っています。

## 解答例

以下を大分参考にさせてもらいました。

- [A Tour of Go で Go に入門した - ゆううきブログ](http://yuuki.hatenablog.com/entry/2014/02/16/183206)


私の解答は GitHub に。

- [ongaeshi/tour-of-go](https://github.com/ongaeshi/tour-of-go)

演習問題は少し難しいけどGo言語の得意分野がなんとなく分かるのでおすすめです。どうしても分からない時は解答を読んで理解してみるといいです。

## 印象に残ったトピック
### #8 Functions continued
[link](http://tour.golang.org/#8)

```
func add(x, y int) int {
   ….
}
```

ローカル変数、引数、戻り値、どれも Go は後ろに付けます。よく考えられていて色々な恩恵があります。例えば同じ型種類の引数が連続している時は一つだけ指定すればよいです。

### #13 Short variable declarations
[link](http://tour.golang.org/#13)

```
func main() {
    var i, j int = 1, 2
    k := 3
```

基本は var でローカル変数を宣言するのですが、最初の初期化の時だけ`:=`で初期化することが出来ます。結果としてほとんどのローカル変数は`:=`で初期化することになり、コードの見通しがよくなります。どうしても初期化が後回しになる時だけ var を使うのがよさそうです。

この機能は本当によいので、 C# や C++ でも var や auto と一緒に使えるようにして欲しいなぁ。

### #20 For is Go's "while"
[link](http://tour.golang.org/#20)

```
func main() {
    sum := 1
    for sum < 1000 {
         ...
    }
```

ループ用の構文は for しかないです。while っぽいループも for で書きます。

```
func main() {
    for {
    }
}
```

引数無しだと無限ループになります。

### #33 Slicing slices
[link](http://tour.golang.org/#33)

特定範囲のサブ配列を取得することが標準で出来ます。

```
func main() {
    p := []int{2, 3, 5, 7, 11, 13}

    fmt.Println("p[1:4] ==", p[1:4])
    fmt.Println("p[:3] ==", p[:3])
    fmt.Println("p[4:] ==", p[4:])
```

### #36 Range
[link](http://tour.golang.org/#36)

foreach は for + range で表現します。for 1つでループ関連を全て表現しようと頑張っている印象です。

```
func main() {
    for i, v := range pow {
        fmt.Printf("2**%d = %d\n", i, v)
}
```

### #41 Map literals continued
[link](http://tour.golang.org/#41)

Map型(数値以外をインデックスに取れる配列、RubyのHash)も標準搭載です。リテラル表記もあるのがありがたいです。

```
type Vertex struct {
    Lat, Long float64
}

var m = map[string]Vertex{
    "Bell Labs": {40.68433, -74.39967},
    "Google":    {37.42202, -122.08408},
}
```

### #44 Function values
[link](http://tour.golang.org/#44)

関数も値としてとれます。クロージャにも対応しているので JavaScript で出来ることもある程度出来るのではないかと思います。

```
func main() {
    hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }
```

### #52 Methods
[link](http://tour.golang.org/#52)

メソッドの書き方はなかなかに個性的で struct の横に書きます。C# や C++ が struct の下に直接関数を定義する事で ``(v *Vertex)`` を省略出来るのでここだけ見ると面倒なのですが・・

```
type Vertex struct {
    X, Y float64
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

### #53 Methods continued
[link](http://tour.golang.org/#53)

float64 を typedef してそこに独自のメソッドを定義するようなことがあっさりと出来ます。

C# や C++ ではメソッドはクラスの所有物なのに対して、Go では構造体とメソッドが対等な関係になります。一長一短ですが基本型にもメソッドが定義出来る事、コンパイラの内部構造をよりすっきりと表現出来ていることは Go のメリットだと考えます。

```
type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}
```

### #57 Interfaces are satisfied implicitly
[link](http://tour.golang.org/#57)

個人的に一番面白かったのはインターフェース周りでした。継承関係を明示しなくても関数定義が条件を満たしていればそのインターフェースに属していることになります。

分かりにくいですが、独自に定義した Writer インターフェースに標準ライブラリの os.Stdout を代入することが出来ます。(Write関数を持っているから) さらに、 fmt.FPprintf(w io.Writer ..) の第1引数に独自に定義した Writer インターフェースを渡す事も出来ます。

```
type Writer interface {
    Write(b []byte) (n int, err error)
}

func main() {
    var w Writer

    // os.Stdout implements Writer
    w = os.Stdout

    fmt.Fprintf(w, "hello, writer\n")
}
```

標準ライブラリもこの機能をフルに使っていて、ユーザーがカスタマイズしたものを渡したいようなものはあらかじめインターフェースで定義されているので、条件を満たす関数を定義すれば簡単に標準ライブラリに渡すことが出来るようになります。詳しくは以下にたくさん載っているので読んでみるとよいです。

- [Big Sky :: Golang のオフィシャルが提供するインタフェースまとめ](http://mattn.kaoriya.net/software/lang/go/20140501172821.htm)

### #65 Goroutines
[link](http://tour.golang.org/#65)

並列化が簡単に書けるのでいいですね。

```
func main() {
    go say("world")
    say("hello")
}
```

## まとめ
[Rebuild: 42](http://rebuild.fm/42/) でも Go言語が紹介されていたし、よいタイミングで勉強出来たような気がします。

どこかで「色々な言語を触っているとGo言語のよさが分かる」というのを見ましたが、まさにその通りだと思います。C/C++/Objective-C/D/C#/Java といった C系の言語と Ruby/Perl/Python/PHP といった LL系の言語を両方触ったことがある人が Go を触ると、両者のよい所を上手く取り入れた言語になっているのが良く分かるのではないでしょうか。

``go run``でコンパイル無しで実行出来たり、ヘッダが不要で記述量が少ないこともあって初見でスクリプトに近い印象も受けるのですが、<b>スクリプトの良さを取り入れたC言語</b>だという理解をしています。さくっと書く時のスピード感はやはりスクリプトの方が早いです。が、C と比べたら Go の快適さは素晴らしいです。

C で書く領域を全て Go で書けるような未来はまだ先でしょうが、今までなら C で書くようなケースで Go で書ける環境が整っているのであれば、試してみる価値は充分にあるのではないのかと思いました。はこべブログでも言及されていましたが LL から使うミドルウェア部分なんかはまさにそういう所だと思います。

ハッシュ、文字コード変換、http とのやり取り、並列処理などが標準として用意されているのも大きいです。この辺を C で書こうとすると Windows, OSX, Linux で分岐するコード書く必要がどうしても出てしまうので。Go ならこの辺りの機能を含んだバイナリを簡単に作る事が出来ます。

他の人の見解もたくさんあるので是非。私の感想よりおすすめです。

- [Go言語の気に入ったところ/気に入らなかったところ - はこべブログ ♨](http://hakobe932.hatenablog.com/entry/2013/12/21/143305)
- [Rebuild.fm ep42の補足等 : D-7 <altijd in beweging>](http://lestrrat.ldblog.jp/archives/38563300.html)
- [規模の大きい本番システムをGo言語で書き直した感想 - ワザノバ | wazanova](http://wazanova.jp/post/66645562844/go)

