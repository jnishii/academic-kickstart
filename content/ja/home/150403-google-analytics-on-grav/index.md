---
title: "grav で google analytics"
date: 2015-04-03
categories: ["comp"]
tags: ["grav"," google analytics"]
---

google analytics でサイトアクセスが0になっていておかしいなと思い，grav に
google analytics をしこむのを忘れていたことに気づいた。
やることは以下のとおり。

<!--more-->
1. `grav/user/themes/mytheme/templates/partials/`にgoogle analyticsのトラッキングコードをコピペする(`mytheme`は自分のカスタムテーマ名)。例えば，こんなかんじ。
``` twig
$ cat google-analytics.html.twig
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', '??-???????-?', 'auto');  <= ??はgoogle analyticsの割当コード
  ga('send', 'pageview');
</script>
```
2. `grav/user/themes/mytheme/templates/partials/base.html.twig`の`<body>`行の直後に以下を挿入。
``` twig
{\% include 'partials/google-analytics.html.twig' \%}
```
