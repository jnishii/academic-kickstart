---
title: "Macのsedでメタ文字が使えなくて困った。"
date: 2015-01-22
categories: ["comp"]
tags: ["mac","sed"]
---

Macのsedでメタ文字 `\+`が使えなくて困ったが，gnu-sedをインストールすればいいことがわかった．めでたい．
<!--more-->

```
$ brew install gnu-sed
```

コマンド名は`gsed`
