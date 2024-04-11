---
Title: Rustをはじめる
Category:
- 開発環境
Date: 2018-01-25T23:58:06+09:00
URL: https://ongaeshi.hatenablog.com/entry/start-rust
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812340892485
---

無性に新しい言語を覚えたくなった。

## インストール
[インストール · プログラミング言語Rust](https://www.rust-lang.org/ja-JP/install.html)

```
$ rustc --version
rustc 1.23.0 (766bd11c8 2018-01-01)
```

ローカルドキュメントを開く。

```
$ rustup doc
```

## チュートリアル
[The Rust Programming Language](https://doc.rust-lang.org/book/)

日本語版もある。Second editionがおすすめみたい。

## Hello, World!
[Hello, World! - Hello, World! - The Rust Programming Language](https://doc.rust-lang.org/book/second-edition/ch01-02-hello-world.html)

```rust
fn main() {
    println!("Hello, World!");
    println!("こんにちは世界！");
}
```

Cargo便利。

## 他の人が作ったプロジェクトをビルド
[Aaronepower/tokei](https://github.com/Aaronepower/tokei)をビルドしてみる。

```
$ git clone https://github.com/Aaronepower/tokei.git
$ cd tokei
$ cargo build
```

Cargoのおかげで簡単。実行。

```
ongaeshi@DESKTOP-7M9KJ3L MSYS /c/Users/ongaeshi/Documents/rust-test/hello_carg                                  o
$ ../tokei/target/debug/tokei.exe
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
 Rust                    1            4            4            0            0
 TOML                    1            6            5            0            1
-------------------------------------------------------------------------------
 Total                   2           10            9            0            1
-------------------------------------------------------------------------------
```



