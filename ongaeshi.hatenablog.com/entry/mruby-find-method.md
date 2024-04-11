---
Title: mrubyであるクラスに特定の名前のメソッドが存在しているか調べる方法
Category:
- diary
- rubypico
Date: 2016-03-15T00:15:11+09:00
URL: https://ongaeshi.hatenablog.com/entry/mruby-find-method
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/10328537792367098953
---

次のPictRubyで以下のような対話型のプログラムが簡単に書けるようになる。

[f:id:tuto0621:20160315001321j:plain]

スクリプトはこんな感じ。(書き方は模索中、そのうち変わるかも)

```ruby
def chat(input)
  @num ||= 0

  case input
  when "name", "Name"
    "My name is Rubo"
  when "Rubo"
    "Yes, my load."
  when "help", "Help"
    <<EOF
Echo your message

name: teach my name
halt: stop this program
EOF
  when "halt", "Halt"
    aaaa
  else
    @num += 1
    "#{@num}: #{input}"
  end
end
```

今までのプログラムと同居するために、スクリプトにchatとmainどちらの関数が定義されているかを調べて、生成するViewControllerを切り替える必要が出てきた。

いろいろと試行錯誤の結果以下のようになった。

```objc
+ (id) NewWithScriptName:(NSString*)scriptPath
{
    mrb_state* mrb = [self InitMrb:scriptPath];

    // ScriptController or ChatViewController
    mrb_sym mid = mrb_intern_cstr(mrb, "chat");
    struct RProc* m = mrb_method_search_vm(mrb, &mrb->object_class, mid);

    if (m) {
        return [[ChatViewController alloc] init:scriptPath mrb:mrb];
    } else {
        return [[ScriptController alloc] init:scriptPath mrb:mrb];
    }
}
```

`mrb_intern_cstr`と`mrb_method_search_vm`の組み合わせで調べられる(&classなことに注意)。最初、

```objc
struct RProc* m = mrb_method_search_vm(mrb, &mrb->kernel_class, mid);
```

ってやっても全然見つからなくて、そうかグローバル関数(本当はそんなものRubyにはないのだろうけど)はKernelじゃなくてObjectに所属するのか、ってなった。mrb_funcallの時は中でうまくmessod_missingしてくれているのかな？
