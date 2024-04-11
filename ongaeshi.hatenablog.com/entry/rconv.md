---
Title: rconv - Ruby WASMでプログラム電卓を作りました
Date: 2022-09-07T22:41:58+09:00
URL: https://ongaeshi.hatenablog.com/entry/rconv
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/4207112889915709104
---

https://rconv.ongaeshi.me

[f:id:tuto0621:20220906231612g:plain]

Ruby on Browser に続いて Ruby WASM/WASI で電卓っぽいものを作りました。
[https://ongaeshi.hatenablog.com/entry/rubyonbrowser-1.0.0:embed:cite]

テキストボックスの内容を入力としてRubyスクリプトに渡して評価し実行結果を出力します。つまり**入力→変換→出力するRubyアプリケーションがブラウザ上で簡単に作れます**。コードはURLに記録されるので[簡単に共有](https://rconv.ongaeshi.me/?q=MQAgFgLhAODOBcB6RBHAlmiBDAdAYwHsBbRAazFlMUwFMjZEATARgFYAzV1gBgDYOATHl7csWACzcBjAFB4ANlliwQAWSzQZIEAEEAMgEkAagFEQAXhDMtIACImdt7Ze4ybjGuxAKaWAE422qDQAK4QsABEADo0ANoAEjGxAgBWETY0AHay7p4gaJmYaFjyaABeNAAUAB4AciFEADQgAJ71TfmwAEpY2cQWIOwlsDQAlIHaAAJEGgOxALoTIJN1DQOrREuTbWuWO5va2pXcOKcbozi%2BeGAgAN4APmj3S8sz0LFo83OLh4fHpzh9hcrjcHilnr9ftMNB95rEUl9LGhur1GP0APwgPyoyr6YxmADUVlGIHgdgcTlAgCpzQABdoArBkAdgyAcwZsdlKgJxpCQABfCa87RZHLaDxeADmNAgAGVsBAqmhmilOYdQIA9hkAJQyAKYZAJ0MgHsGQCyiYB0JUASQyAGP1ALEMgGkGQCRDIBaqMAAQzMwryQCDDIByhkAwwyAeoZABYMgCCGQAOrnTANEMgCAGQASioBrBkAJmmANE0JmgvGgQAAeEDcED3e4gFIptMvPwSkJ%2BTLkxwTQVuX4J14wz44J3ol6l2zl%2BSwavQ96feHzetoeSNrnN1sjJud2E98vZDLT3JiiWShoADlqNDQorAACMCH4wAQCIxKvLs0rtLBdnnfueiANryAieKpTK5SAALRWBWnkB3yx3h8L58j3vD8TwmH9vzWf8nywWUj2aX4c3fZgv3Av8QEfaUYLleDDhzIlkLAi80IwwCkyQhU32JQib1-SD0IArCgPwiikJQoi6JIxiyJAvCqKvdibygzDYKTZjs2AgiBVnYU8hCaBGCwiZMhoAB3dRoG%2BCZ-lOFZ2mBLBrjuR4IUhZS1NrREQAWF5tJwbY9MuAzQXucEmxAMz1InBEBjwEo8FXFThOw0CuX5Q4wqmN4Bg8jQZyFEARW8PyAqCuCQsOWBnwGcduwRaiBgw5dV3XLcdz3A80sVeMvAyrLzEsexHBAAAyZqIJoywAGYmzxUwR2rTKsIsSxerMVqQEqH9LAEDMsymkBOq-Q5Rv6rxBtlYbdEMUwWraya1mTSxJK5RqW1%2BGg2wGuqRu2sa2rvAA%2BSxxCbU6RxoJt1o%2B87pyk%2BLEsYbEVImHx-C0k4dPORzrgeJ4bIhuygWhsAwRMrkOzeLzEQaik3O0aA-AKCAQAiAB9dIhxAC7R0p-HCcyYmIkmCmhwrUKXlCcIQAAcm5vk4srBK8hGaCRIVZovqWnK4W839nwFudOhMaoIAKPBVYITIlmAABCTscHYRQoCyHA72vHUBBwcRrSZQB1BkAfQYQ0AGwYLUAYwZ7a2N4DaN2VMhwAoUhodXKngAkSXqy8qenCsZCiyx1PrVTKiXRoRGaCA-BCMYZBLI63C6QhMgAN1NiVKlViB5BoMkIgLzWi4iZoRSwEJ5AgMlkISggMxoEzqxoTbXF%2BOO1A0ROVOT1PuHTzOxhAJSBmsAU22%2Bw4SwJPPh7HuSFNlBX8fcmwvcBrBgYrIA)することができます。

## 基本
`Rconv.set()`メソッドに渡したブロックの中身が変換プログラムになります。ブロック引数に渡されるのはテキストボックスの内容がevalされたものです。`Always string = true` のときは常に文字列になります。ブロックの戻り値、もしくは標準出力に何か出力されたときはその内容が、そうでないときはブロックの戻り値が出力されます。

例えば irb 風の挙動を作りたいときはこんな感じ。(入力途中で出てくるエラーメッセージも大変分かりやすくてここはCRubyの蓄積が効いているなぁと思います)

[f:id:tuto0621:20220907223655g:plain]

```ruby
Rconv.set(title: "Rconv", default: 1) do |e|
  p e
end
```

自分はRubyでいくつかのキーワードをまとめて置換するテキストフィルターみたいなやつをよく書くのですが、そういうのも得意です。

```ruby
Rconv.set(title: "テキスト置換", default: 1) do |e|
  e.gsub(/\bProgress\b/, "Progressssss")
  .gsub(/@color\b/, "@colorrrr")
  .gsub(/@tty\b/, "@ttyyyyyy")
end
```

## 応用
10進数を16進数、2進数に変換したり、

[f:id:tuto0621:20220907223703g:plain]

```ruby
def to_hex_bin(num)
  puts num
  puts "0x" + num.to_s(16)
  puts "0b" + num.to_s(2)
end

Rconv.set(title: "10進数を16進数、2進数に変換",
          default: 4660) do |e|
  to_hex_bin(e)
end
```

byteからKiB, MiB, GiBに変換とか。

[f:id:tuto0621:20220906231650g:plain]

```ruby
def b2k(byte, n)
  (byte / (2 ** n).to_f).round(2)
end

def puts_b2k(byte, n, unit)
  v = b2k(byte, n)
  puts "#{v} #{unit}" if v != 0
end
  
def byte_to_kib(byte)
  puts "#{byte} byte"
  puts_b2k(byte, 10, "KiB")
  puts_b2k(byte, 20, "MiB")
  puts_b2k(byte, 30, "GiB")
  puts_b2k(byte, 40, "TiB")
  puts_b2k(byte, 50, "PiB")
  puts_b2k(byte, 60, "EiB")
end

Rconv.set(title: "byteからKiB, MiB, GiBに変換") do |e|
  byte_to_kib(e)
end
```

またRuby/WASMはCRubyなのでちまたにあるRubyスクリプトを移植することができます。自分は昔作った[caseninja](https://github.com/ongaeshi/caseninja)というgemを移植しました。(変数名を任意のケースに変換することができるプログラム) 結構愛用しています。

[f:id:tuto0621:20220906231704g:plain]

(これは長いのでリンク貼ります) [Caseninja -rconv](https://rconv.ongaeshi.me/?q=LYewJgrgNgpgBAYQIYGcYDsCW6BWSBQccoksA%2BgGYToDGALpiOvoXGDBXHSGQBaq8AFHRgAPOgEpWRAN7SicdCABOwJFABcXHktXrhYyQBp5RGv2xbuZc0mwHxEkwoUp0SANYwrPN55gOxqZwNEjAMJraNmERgU7BAA6ooZHWSSgpcc4ucBAJtpZReQXoWcF5fl4%2BZBXuXmUuAL6sGGAsROyc1rpqUHHyPeqYAF4BIo4t6G2snVEl-S6DUCNjhhIAdADmKBAARoIARHAHRscAtAdSRK3tbBxRlasTiyq9K3FbO-tHJ8dkl5Nph17tZQuE%2BuNJPI0sl9JCNl9BAB6dZIiRwGRwAAkADJ1mAQAB3WioeDNa5TW6zGEZOFrAavIajD4oBLLOjrNQJDFwAA%2BMF5cBg61CCUwdCZZPWOBA2EBVJBPGKFlK8OhPHm8PWxVJ8qIM0VNQSjwWCmsJq1OrQ8oNnCW7zVLkhcAAvFxDOsUHRlJgEvJ5JguoZXQA-OBI8PBSH4okk60uCIoQPu8Sh8NnJFRj3bPaHC6nH4bAnE0LxhSJ5POl1hpFkTM5FMcnPff4F45F2OlmDyRPdhvR1ns5GCAD8LoA2gBBM4ALQAuhI0dLZaVCzGS7qE5SKW0bvgAEo0JgAN09MDownFsC0B2QaCwuCQv06SGgdBvFBAIAAQkhlL-hl%2BdRCSQABPFAyC9H10E2KxlAgGB0QJPkBVYBIIDoFBEFJB88HWax%2BBQIREPWY91AQlB8BuIA)

## おわりに
[rconv/sample.ja.md at main · ongaeshi/rconv](https://github.com/ongaeshi/rconv/blob/main/sample/sample.ja.md)にここで紹介したサンプルコードも置いてあるので是非遊んでみてください。よいものができたら #rconv とかで教えてくれると喜びます。

