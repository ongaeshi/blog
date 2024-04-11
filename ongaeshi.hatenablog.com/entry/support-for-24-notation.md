---
Title: radiko.rbで時間表記を"Mon 25:00"と書けるようにした
Category:
- rubypico
Date: 2017-08-14T23:39:59+09:00
URL: https://ongaeshi.hatenablog.com/entry/support-for-24-notation
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8599973812287681391
---

ドッグフーディングしながらコツコツと改良を加えている[radiko.rb](https://github.com/rubypico/radiko)。

深夜のラジオ番組は大体

- "Mon 25:00" (本当は火曜日の深夜1:00)
- "Sat 24:00" (本当は日曜日の深夜0:00)

のような表記をされることが多い。今までは"Mon 25:00" -> "Tue 1:00" のように変換して表記する必要があったのだがこれをどちらでも書けるようにした。

[https://github.com/rubypico/radiko/commit/597c2fb79cc08ba9bddbe0e0a2dc378d44b96a20:embed:cite]

```ruby
require "radiko/radiko"
include Radiko

c "ハライチのターン！", "Thu 24:00", %w(TBS RBC)
c "オレたちゴチャ・まぜ", "Sat 25:50", %w(MBS)
```

番組表そのままで記述できるので便利。
