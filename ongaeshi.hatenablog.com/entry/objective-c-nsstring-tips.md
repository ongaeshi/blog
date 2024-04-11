---
Title: Objective-Cの文字列操作あれこれ
Category:
- objective-c
- openframeworks
- ruby
- csharp
- ofruby
Date: 2014-08-12T11:07:26+09:00
URL: https://ongaeshi.hatenablog.com/entry/objective-c-nsstring-tips
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815730059690
---

iOS(とOSX)の文字列は[NSString](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)で`@"...."`で表現する。基本的にObjective-Cが拡張した構文には`@`が付く。C言語の構文と見分けを付きやすくするためだろうか。

NSStringの便利さはC言語の標準ライブラリであれこれやるよりは楽だけどC#やRubyと比べると少し不便な印象。あとメソッド名が長い。でもまあ慣れてくれば何とかなりそう。

## ファイルパスから一番後ろの要素を取得
Rubyの`File.basename`に相当するやつ。

```objective-c
NSString* str = [@"/path/to/file.c" lastPathComponent]; //=> @"file.c"
```

## 拡張子を取得

`.`は付かないので注意。

```
NSString* extension = [@"/path/to/file.c" pathExtension]; //=> @"c"
```

## 拡張子を削除

```objective-c
NSString* str = [@"/path/to/file.c" stringByDeletingPathExtension]; //=> @"/path/to/file"
```

## 応用1: ファイル名を抜き出して拡張子がついていなければ必ず付ける

[ofruby](https://github.com/ongaeshi/ofruby-ios/blob/4c6c82601047fbf9eb4652573c82a52a75307cf6/apps/myApps/ofruby/src/SelectViewController.mm#L60)で使ったコード。ユーザーの入力に対して常に拡張子を付けて生成したい。`file.c.c`とかならないように一旦拡張子を削除してから再度付けている。

```objective-c
// Remove a directory path and Add the ".rb" extension.
NSString* str = [[[@"/path/to/file" lastPathComponent] stringByDeletingPathExtension] stringByAppendingString:@".c"]; //=> @"file.c"
NSString* str2 = [[[@"/path/to/file.c" lastPathComponent] stringByDeletingPathExtension] stringByAppendingString:@".c"]; //=> @"file.c"
```

## 応用2: 渡されたパスをファイル名と拡張子に分解する
[objective c - pathForResource? without using extension (Iphone) - Stack Overflow](http://stackoverflow.com/questions/1984572/pathforresource-without-using-extension-iphone)

NSBundleのpathForResourceにアクセスする時は(何故か)ファイル名と拡張子を別々に指定しないといけない。で、`ofType`(拡張子)を使わずに指定する方法はないか？っていう質問に返ってきたのがこれ。まあ確かにそうなんだけど出来れば分けずに指定するAPIが欲しいところ。

```objective-c
NSString* fullFileName = @"image.jpg";
NSString* fileName = [[fullFileName lastPathComponent] stringByDeletingPathExtension];
NSString* extension = [fullFileName pathExtension];

[[NSBundle mainBundle] pathForResource:fileName ofType:extension]];
```

## 今日の成果
ファイル名を指定して作成出来るようになりました。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140812/20140812110705.png" alt="f:id:tuto0621:20140812110705p:plain" title="f:id:tuto0621:20140812110705p:plain" class="hatena-fotolife" itemprop="image"></span></p>
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140812/20140812110711.png" alt="f:id:tuto0621:20140812110711p:plain" title="f:id:tuto0621:20140812110711p:plain" class="hatena-fotolife" itemprop="image"></span></p>
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140812/20140812110717.png" alt="f:id:tuto0621:20140812110717p:plain" title="f:id:tuto0621:20140812110717p:plain" class="hatena-fotolife" itemprop="image"></span></p>
<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20140812/20140812110723.png" alt="f:id:tuto0621:20140812110723p:plain" title="f:id:tuto0621:20140812110723p:plain" class="hatena-fotolife" itemprop="image"></span></p>




