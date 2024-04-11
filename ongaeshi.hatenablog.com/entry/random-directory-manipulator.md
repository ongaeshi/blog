---
Title: ' Rubyでランダムにディレクトリの中身を書き換えるライブラリを作った'
Category:
- ruby
- groonga
Date: 2014-03-17T10:22:21+09:00
URL: https://ongaeshi.hatenablog.com/entry/random-directory-manipulator
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815720043996
---

[Groongaのデータベース肥大化をテスト](http://www.clear-code.com/blog/2014/3/13.html)しようと思って、

1. テスト用のGroongaを仮想環境にインストール
1. Milkodeデータベースを作成 (milk init)
1. 適当なソースコードをデータベースに追加(milk add)
1. **ディレクトリの中身を更新、追加、削除で適当に書き換える**
1. データベースを更新(milk update)
1. 4, 5 を任意の回数繰り返す

という方法でやろうと思ったのですが、4の<b>ディレクトリの中身を書き換える</b>ライブラリが見つからなかったので作成しました。

※ 思いついてから動かすまで1時間位で出来た、Ruby素晴らしい。

## 使い方
ディレクトリを指定して生成し、メソッドを呼ぶだけです

```ruby
# 書き換えるディレクトリを指定
rdm = RandomDirectoryManipulator.new('dir')

# 追加、更新、削除を1000回繰り返す
rdm.add_remove_update(1000)
```

乱数の種やデバッグ表示を表示することが出来ます。乱数の種を固定すると、書き換えるディレクトリの内容が同じであれば、常に同じ書き換え処理を行うことが出来ます。

```ruby
# 乱数の種 = 1 で実行
rdm = RandomDirectoryManipulator.new('dir', {seed: 1})

# デバッグ表示ON
rdm = RandomDirectoryManipulator.new('dir', {verbose: true})
```

## 実行結果
verbose=true, 10回繰り返しだと以下のようになります。

```ruby
rdm = RandomDirectoryManipulator.new('dir', {verbose: true})
rdm.add_remove_update(10)
```

```bash
$ ruby ./sample.rb
U dir/ext/tk/lib/tkextlib/iwidgets/dialog.rb
U dir/ext/stringio/stringio.c
A dir/test/json/fixtures/pass1.json-random_directory_manipulator
R dir/benchmark/other-lang/fib.pl
U dir/tool/change_maker.rb
U dir/test/ruby/test_complex.rb-random_directory_manipulator
U dir/ext/tk/sample/demos-jp/ctext.rb
U dir/test/open-uri/test_open-uri.rb-random_directory_manipulator
```

U(更新), A(追加), R(削除) です。
Uの場合はファイルの末尾に`random_directory_manipulator`を追記します。
Aの場合はファイル名に`-random_directory_manipulator`を付けてコピーします。
Rはそのままファイルを削除します。

更新(80%)、追加(10%)、削除(10%) なのでファイル数はあまり変わらずに少しずつファイルの総容量が増えていくイメージです。ここの確率をいじると色々とエミュレーション出来るかもしれません。

## ソースコード
[ongaeshi/groonga-broated-test](https://github.com/ongaeshi/groonga-broated-test)/Rakefile に実体があります。

```ruby
class RandomDirectoryManipulator
  def initialize(dir, options = {})
    @dir = dir
    @options = options
    srand(@options[:seed]) if @options[:seed]
  end

  def add_remove_update(num)
    files = dir_files
    
    (1..num).each do
      file = files[rand(files.size)]

      case rand(10)
      when 0
        # Add
        dst = file + "-random_directory_manipulator"
        message "A #{dst}"
        FileUtils.cp(file, dst)
        files << dst
      when 1
        # Remove
        message "R #{file}"
        File.unlink(file)
        files.delete(file)
      else
        # Update
        message "U #{file}"
        open(file, "a") do |io|
          io.puts "random_directory_manipulator"
        end
      end
    end
  end

  def dir_files
    files = []
    
    Find.find(@dir) do |path|
      next if FileTest.directory?(path)
      files << path
    end

    files
  end

  def message(str)
    puts str if @options[:verbose]
  end
end
```

