---
Title: blogsync で Windows からも直接新規記事を投稿したい
Category:
- blogsync
- programming
Date: 2024-04-08T00:30:00+09:00
URL: https://ongaeshi.hatenablog.com/entry/blogsync-windows-direct-post
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/4207112889981771582
---

[https://github.com/x-motemen/blogsync?tab=readme-ov-file#%E3%82%A8%E3%83%B3%E3%83%88%E3%83%AA%E3%82%92%E6%8A%95%E7%A8%BF%E3%81%99%E3%82%8Bblogsync-post:embed:cite]

を Windows からもやりたい。

PowerShell で標準入力を終了するにはCtrl+Z Enter なので・・

```
PS> ~/Documents/blog
PS> blogsync post --draft --title=blogsync ongaeshi.hatenablog.com
さてかきはじめるか...
(Ctrl+Z Enter)
```

これで OK。

[blog:g:11696248318754550880:banner]
