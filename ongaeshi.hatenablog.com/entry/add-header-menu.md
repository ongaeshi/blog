---
Title: はてなブログのデザインを改造してヘッダーメニューを付けてみた
Category:
- web
- blog
- html
- css
Date: 2015-05-23T01:37:37+09:00
URL: https://ongaeshi.hatenablog.com/entry/add-header-menu
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/8454420450095233457
---

関連記事を見やすくするためにカテゴリメニューを付けた。記事数の多いカテゴリとホームページとTwitterへのリンクを貼った。

- [はてなブログのヘッダーにナビゲーションをつけよう！ - 現代版徒然草](http://rrt.hateblo.jp/entry/2014/03/05/030640)

を参考に背景色だけ調整(シンプルにした)。


## ソースコード

カスタムヘッダ。

```html
<nav id="global">
  <ul>
    <li><a href="http://ongaeshi.hatenablog.com/archive/category/ofruby">ofruby</a></li>
    <li><a href="http://ongaeshi.hatenablog.com/archive/category/ruby">Ruby</a></li>
    <li><a href="http://ongaeshi.hatenablog.com/archive/category/milkode">Milkode</a></li>
    <li><a href="http://ongaeshi.hatenablog.com/archive/category/emacs">Emacs</a></li>
    <li><a href="http://ongaeshi.hatenablog.com/archive/category/groonga">Groonga</a></li>
    <li><a href="http://ongaeshi.me">Home</a></li>
    <li><a href="https://twitter.com/ongaeshi">Twitter</a></li>
  </ul>
</nav>
```

カスタムCSS

```css
/* <system section="theme" selected="report"> */
@import "/css/theme/report/report.css";
/* </system> */

/* <system section="background" selected="208d44"> */
body{background:#c0f0cf;}
/* </system> */

#container {
    background: none repeat scroll 0 0 #FFFFFF;
    margin: 0 auto;
    padding: 0 30px;
    text-align: center;
    /*width: 890px;*/
    width: 720px;
}

#wrapper {
    float: left;
    width: 720px;
}

.entry-header {
    border-bottom-color: #CCCCCC;
    border-bottom-style: solid;
    border-bottom-width: 1px;
}

.entry-title a {
    font-size: 38px;
}

.entry-content {
    font-size: 16px;
}

.categories {
    visibility: hidden;
}

/* ここから下が今回追加部分 */

#blog-title {
    padding-bottom: 10px;
}

#title {
    /*font-size: 38px;*/
    /* background-image: url("http://cdn1.www.st-hatena.com/users/tu/tuto0621/profile.gif") */
}

#top-editarea {
    width: 100%;
    text-shadow: inherit;
    padding-bottom: 30px;
}

#global ul {
    list-style: none;
    width: 100%;
    text-align: center;
    border-top: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
    /* background: #fff; /\* Old browsers *\/ */
    /* background: -moz-linear-gradient(top, rgba(255,255,255,1) 0%, rgba(246,246,246,1) 47%, rgba(237,237,237,1) 100%); /\* FF3.6+ *\/ */
    /* background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(255,255,255,1)), color-stop(47%,rgba(246,246,246,1)), color-stop(100%,rgba(237,237,237,1))); /\* Chrome,Safari4+ *\/ */
    /* background: -webkit-linear-gradient(top, rgba(255,255,255,1) 0%,rgba(246,246,246,1) 47%,rgba(237,237,237,1) 100%); /\* Chrome10+,Safari5.1+ *\/ */
    /* background: -o-linear-gradient(top, rgba(255,255,255,1) 0%,rgba(246,246,246,1) 47%,rgba(237,237,237,1) 100%); /\* Opera 11.10+ *\/ */
    /* background: -ms-linear-gradient(top, rgba(255,255,255,1) 0%,rgba(246,246,246,1) 47%,rgba(237,237,237,1) 100%); /\* IE10+ *\/ */
    /* background: linear-gradient(to bottom, rgba(255,255,255,1) 0%,rgba(246,246,246,1) 47%,rgba(237,237,237,1) 100%); /\* W3C *\/ */
}

#global ul li {
    display: inline-block;
}

#global ul li a {
    text-decoration: none;
    display: block;
    padding: 1em;
}

#content {
    margin-top: 0;
    background: #fff; /* ☆ */
    box-shadow: inherit;
}
```
