---
Title: iOSのバージョンによって処理を切り分けるための関数を書いた
Category:
- ios
- ofruby
- apple
- objective-c
Date: 2015-05-18T23:11:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/switch-by-the-ios-version
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450094844743
---

[昨日のシンタックスハイライト](http://ongaeshi.hatenablog.com/entry/builtin-ruby-syntax-hight-to-ios)に必要なTextKitはiOS7以降でしか使えないので、iOS6以前は今までどおり普通の`UITextView`を使っている。iOSのバージョンによって処理を切り分ける関数を書いたのでメモ。

iOS6以前
[f:id:tuto0621:20140811024621p:plain]

iOS7以降
[f:id:tuto0621:20150517175305p:plain]


## isSyntaxHighlight関数
isSyntaxHighlight関数の定義には`NSFoundationVersionNumber`を使う。

```objc
-(BOOL)isSyntaxHighlight
{
    return NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_6_1; // 7.0 and above
}
```

下のように使う。7以上だったらTextStorage, LayoutManager, TextContainerを作ってICTextViewに渡して生成するけど、6以下の時は普通に作るだけ。

[EditViewController.mm:49](https://github.com/ongaeshi/ofruby-ios/blob/feature/code-highlight/apps/myApps/ofruby/src/EditViewController.mm#L49)

```objc
    if ([self isSyntaxHighlight]) {
        // TextStorage
        mTextStorage = [RubyHighlightingTextStorage new];
        [mTextStorage replaceCharactersInRange:NSMakeRange(0, 0) withString:[FCFileManager readFileAtPath:mFileName]];

        // LayoutManager
        NSLayoutManager *textLayout = [[NSLayoutManager alloc] init];
        [mTextStorage addLayoutManager:textLayout]; 

        // TextContainer
        NSTextContainer *textContainer = [[NSTextContainer alloc] initWithSize:self.view.bounds.size];
        [textLayout addTextContainer:textContainer];

        // TextView
        mTextView = [[ICTextView alloc] initWithFrame:self.view.bounds textContainer:textContainer];
    } else {
        // TextView
        mTextView = [[ICTextView alloc] initWithFrame:self.view.bounds];
        mTextView.text = [FCFileManager readFileAtPath:mFileName];
    }
```

参考: [iOSのバージョンによる分岐 - Qiita](http://qiita.com/makoto_kw/items/91b5c21aaf886fd23f5a)
