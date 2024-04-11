---
Title: mrubyにUIButtonをバインド(登録)する
Category:
- diary
- rubypico
Date: 2016-03-08T00:07:54+09:00
URL: https://ongaeshi.hatenablog.com/entry/bind-uibutton-to-mruby
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792366180031
---

[PictRuby](http://pictruby.ongaeshi.me)でUIButtonやUILabelが追加できるようになったら楽しいだろうなぁと思い色々と実験中。ぼんやりと動いてきた。

[PictRuby/mrb_button.m at feature/add-button · ongaeshi/PictRuby](https://github.com/ongaeshi/PictRuby/blob/feature/add-button/PictRuby/mrb_button.m)

以下、細かいポイント

## ARCを切る
retain, release を手動で制御したいので mrb_button.m には`-fno-objc-arc`を設定しておく

[ARCの有効/無効設定を切り替える方法 - Dolice Lab](http://lab.dolice.net/blog/2013/05/10/objc-arc-switch/)

## mrubyにButton型を登録
`MRB_SET_INSTANCE_TT(cc, MRB_TT_DATA);`でインスタンスをデータオブジェクトとして登録する。

```objc
/* Setup & Unsetup */
void
mrb_pictruby_button_init(mrb_state *mrb)
{
    struct RClass *cc = mrb_define_class(mrb, "Button", mrb->object_class);
    MRB_SET_INSTANCE_TT(cc, MRB_TT_DATA);

    mrb_define_method(mrb, cc, "initialize", initialize, MRB_ARGS_NONE());
}

void
mrb_pictruby_button_final(mrb_state *mrb)
{
}
```

## UI生成時にretainを呼ぶ
UIButton自体の生成関数は作成途中。ポイントは生成した時に`retain`を呼ぶこと。
`to_value()`の中身はこの後で。

```objc
/* Create UIButton */
static mrb_value
initialize(mrb_state *mrb, mrb_value self)
{
    UIButton *button = [[UIButton buttonWithType:UIButtonTypeRoundedRect] retain];
    button.backgroundColor = [UIColor clearColor];
    [button setTitle:@"TEST" forState:UIControlStateNormal];
    button.frame = CGRectMake(100.0, 200.0, 100.0, 50.0);
    [globalScriptController.view addSubview:button];

    return to_value(mrb, mrb_class_ptr(self), button);
}
```

## 解放関数にreleaseを設定
`Data_Wrap_Struct`はmrubyにデータオブジェクトをぶら下げる時のAPI。
RubyのGCでオブジェクトが解放されたときに`free_func`が呼ばれる。
`__bridge`はObj-CとC間でポインタをやり取りする時のおまじない。
結果として、初期化時にretainされたカウントが-1され、解放される(はず)。

```objc
/* Setup data_type */
static void
free_func(mrb_state *mrb, void *p)
{
    [(__bridge UIButton*)p release];
}

struct mrb_data_type mrb_pictruby_button_type = { "pictruby_button", free_func };

static mrb_value
to_value(mrb_state *mrb, struct RClass *cc, UIButton *ptr)
{
    if (ptr) {
        return mrb_obj_value(Data_Wrap_Struct(mrb, cc, &mrb_pictruby_button_type, (__bridge void*)ptr));
    } else {
        return mrb_nil_value();
    }
}
```

## 結果
とりあえずボタンらしきものが画面に表示された。

[f:id:tuto0621:20160308000645j:plain]

次はクリックされたときにRubyで書いたブロックを実行できるようにしたい。

```ruby
Button.new do
  Popup.msg "HELLO!"
end
```
