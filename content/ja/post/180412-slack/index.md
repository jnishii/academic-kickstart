---
title: "Slack"
date: 2018-04-12
categories: ["comp"]
tags: ["slack"]
---

研究室メンバとの連絡は[Slack](http://www.slack.com)を使っている。
不要になった古いメッセージなどを一括で消すツールはないかなと探したら，[slack-cleaner](https://github.com/sgratzl/slack-cleaner)というのがあった。

<!--more-->

コマンドラインでいろいろ条件設定して消去でき，テストランもできるので便利。
使うにはPython環境が必要だが，インストールはpipで簡単にできる。

```
$ pip install slack-cleaner
```
