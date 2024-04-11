---
Title: ' mrubyソースコード検索を作りました'
Category:
- mruby
- ruby
- groonga
- milkode
- programming
Date: 2013-10-01T10:59:25+09:00
URL: https://ongaeshi.hatenablog.com/entry/create-mruby-search
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/11696248318758214537
---

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130929/20130929123940.png" alt="f:id:tuto0621:20130929123940p:plain" title="f:id:tuto0621:20130929123940p:plain" class="hatena-fotolife" itemprop="image"></span></p>

http://mrubysearch.ongaeshi.me/

[RubyKokuban](https://github.com/ongaeshi/rubykokuban-gem)を作るにあたってmrubyのソースやmgem(mrubyのRubyGemsみたいなやつ)に登録されたソースコードを簡単に読めるようにしたいなあ、と思い作ってみました。

一日一回レポジトリを最新に更新してインデックスの再構築を行っています。新しいmgemが追加された時は今の所手動で対応しています(そのうち自動化したい所です)。

自分が追加した(もしくは検索したい)mgemが追加されていない！って人はリクエスト頂ければ追加するので教えて下さい。
mgemじゃなくてもmrubyに関係がありそうなソースコードでしたら追加しますので是非。

※ 個人サイトのため突然停止したりするかもしれません、ご了承下さい。

## 使い方の例1 - "!"付きのメソッドはどうやってバインドするの？
例えば`gsub!`のような!マークのついたメソッドはどうやってバインドするのか？を知りたいとします。mrubyでは **メソッド名を文字列で渡す**必要があるのでそこを手がかりにします。`!"`で検索してみましょう(文字列の終端に引っかかることを期待しています)。

[!" in root -mruby search](http://mrubysearch.ongaeshi.me/home?query=!%22&shead=package)

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130929/20130929124240.png" alt="f:id:tuto0621:20130929124240p:plain" title="f:id:tuto0621:20130929124240p:plain" class="hatena-fotolife" itemprop="image"></span></p>

少し下に行くと`swapcase!`関数をバインドしてる箇所が見つかりました。

[mruby-string-ext/src/string.c#173](http://mrubysearch.ongaeshi.me/home/mruby/mrbgems/mruby-string-ext/src/string.c?query=!%22&shead=package#n173)

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/t/tuto0621/20130929/20130929124250.png" alt="f:id:tuto0621:20130929124250p:plain" title="f:id:tuto0621:20130929124250p:plain" class="hatena-fotolife" itemprop="image"></span></p>

`mrb_str_swapcase_bang`という関数名でバインドされていますね。!は **_bang**という名前でバインドされているようです。

他のライブラリではどうなっているか調べてみます。

['_bang' in root - mruby search](http://mrubysearch.ongaeshi.me/home?query=_bang&shead=package)

- `reverse!` -> mrb_ary_reverse_bang
-  `capitalize!` -> mrb_str_capitalize_bang

も_bangという名前になっているので、よく使われているルールだと思ってよさそうです。

## 使い方の例2 - Cの関数内からyieldしたいのだけど
ブロック構文を使いたくなったので調べることに。おそらく`each`という名前のメソッドはブロック構文を使っているのでそれを探してみます。

['each mrb_define_' in root - mruby search](http://mrubysearch.ongaeshi.me/home?query=each+mrb_define_&shead=package)

eachだけでも探せるけどより検索精度を上げるためにmrb_define_キーワードを重ねています。いくつか関数を探して`mrb_leapmotion_devicelist_each`関数を詳しく読む事に決めました。

[mruby-leapmotion/src/Leap.cpp#1067](http://mrubysearch.ongaeshi.me/home/
mruby-leapmotion/src/Leap.cpp?query=mrb_leapmotion_devicelist_each&shead=package#n1067)

```cpp
static mrb_value
mrb_leapmotion_devicelist_each(mrb_state *mrb, mrb_value self)
{
  mrb_leapmotion_devicelist_t *data =
    static_cast<mrb_leapmotion_devicelist_t*>(mrb_data_get_ptr(mrb, self, &mrb_leapmotion_devicelist_type));
  if (data == NULL) {
    return mrb_nil_value();
  }
  mrb_value block;
  mrb_get_args(mrb, "&", &block);
  std::for_each (data->obj->begin(), data->obj->end(),
    mrb_each_func_t<Leap::DeviceList::const_iterator::value_type>(mrb, block));
  return self;
}
```

mrb_get_args_でブロックらしきものを`"&"`で取っているので間違いなさそうです。C++のコードなのでSTLを使っています。`mrb_each_func_t`から始まるテンプレート関数が処理の本体のようです。mrb_each_func_tは同じファイル内にあります。

```cpp
template <typename T> struct mrb_each_func_t {
private:
  mrb_state *mrb_;
  mrb_value &block_;

public:
  mrb_each_func_t(mrb_state *mrb, mrb_value &block) : mrb_(mrb), block_(block) {}
  ~mrb_each_func_t() {}

  mrb_value operator () (T const &elem) const {
    return mrb_yield(mrb_, block_, mrb_leapmotion_obj_make(mrb_, elem));
  }
};
```

`operator ()`内にある **mrb_yield** がブロックを呼び出す関数のようです！長い道のりでしたが、

1. **mrb_get_args(mrb, "&", &block);** でブロックを受け取る
1. **mrb_yield(mrb, block, value)** で受け取ったブロックを実行

すればよいことがmurbyソースコード検索を使って分かりました！

その他の基本的な使い方は[ヘルプ](http://mrubysearch.ongaeshi.me/help)をどうぞ。

## 内部の話
### 構成
- フロントエンド: [Milkode](http://milkode.ongaeshi.me/)
- 検索エンジン: [rroonga](http://ranguba.org/ja/) + [groonga](http://groonga.org/ja/)
- ストレージ: groonga

groongaがストレージエンジンも兼ねるのでMySQL等は使っていません。

### groongaのCentOSへのインストール

先にgroonga-develをインストールしておくとrroongaのインストールが短く終わります。

```
 $ yum install groonga-devel -y
 $ gem install rroonga
```

### Milkodeのサーバーへのセットアップ
milkode-webを使いました、bundlerで管理出来るのでとっても便利です。

**milkode-web - Milkode web application bundler template for apache passenger**
https://github.com/y-ken/milkode-web

くわしくは[groonga(rroonga)を利用したソースコード全文検索エンジン"Milkode"をApache Passengerで軽快に動かす方法](http://y-ken.hatenablog.com/entry/how-to-install-milkode-with-passenger)をどうぞ。

### レポジトリ一覧の取得
mrubyのレポジトリはGitHubから追加します。

```bash
$ milk add  https://github.com/mruby/mruby
```

mgemのレポジトリ一覧は以下のようなスクリプトを使って取得しています。

```ruby
# Create mgem repository list

require 'yaml'

mgem_dir = File.join(ENV['HOME'], '.mgem')
mgem_list = Dir.glob(File.join(mgem_dir, 'mgem-list/*.gem'))

repositories = mgem_list.map do |mgem|
  YAML.load(File.read(mgem))['repository']
end

repositories.each do |repo|
    puts "milk add #{repo} -p git"
end
```

コマンド文字列が生成されるので後はコピペするなりシェルスクリプトにまとめてサーバー上で実行します。

```shell
$ ruby mklist.rb 
milk add https://github.com/schmurfy/host-stats.git -p git
milk add https://github.com/cremno/mruby-allegro.git -p git
milk add https://github.com/ppibburr/mruby-allocate.git -p git
.
.
```


