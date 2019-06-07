---
title: "日付を挿入するキーマップをAtomに追加"
date: 2015-02-21
categories: ["comp"]
tags: ["atom"]
---

[Atom](http://atom.io)に日付挿入用キーマップ`alt-shift-D`を追加した。
<!--more-->

1. packageに"Date & Time"を追加
2. フォーマットをYYYY/MM/DDにしたいので，READMEを見たら"オプション指定でできる"とあるが，指定の仕方がわからない。仕方ないのでPackage/Date の "View code"を見たら lib/date.js にフォーマット指定できるところがあったので，そこを修正した。まぁいいか。
3. keymap.jsonに以下を追加
```
    'body':
        'alt-shift-D': 'date:date'
```
