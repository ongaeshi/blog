---
Title: blog new コマンドの実装
Date: 2024-07-06T20:59:58+09:00
URL: https://ongaeshi.hatenablog.com/entry/2024/07/implement-blog-new-command
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/6801883189119939549
---
引数は `--custom-path` に渡すことで URL を直接指定できるようにする。
`--draft` と `--title` はオプション。

```ruby
class Main < Thor
  desc "new PATH", "Create a new blog post with PATH"
  method_option :title, type: :string
  method_option :draft, type: :boolean
  def new(path)
    Dir.chdir(BLOG_REPOSITORY_DIR) do
      opts = []
      opts << "--title=\"#{options[:title]}\"" if options[:title]
      opts << "--draft" if options[:draft]
      system("blogsync post --custom-path #{path } #{opts.join(" ")} ongaeshi.hatenablog.com")
    end
  end
```
