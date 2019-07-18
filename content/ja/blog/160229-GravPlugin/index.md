---
title: "文字色を変えるGravのプラグイン"
date: 2016-02-29
categories: ["comp"]
tags: ["grav"," cms"]
---

このホームページ作成に使っている[Grav](http://getgrav.org)はMarkdown記法。
Markdownには文字色指定タグがないのだけど，プラグインが2種類ある。

<!--more-->

### [Markdown Color](https://github.com/getgrav/grav-plugin-markdown-color)
```
{c:red}これは赤で{/c}，{c:#0000ff}これは青だよ{/c}。
```
{c:red}これは赤で{/c}，{c:#0000ff}これは青だよ{/c}。

### [Shortcode Core](https://github.com/getgrav/grav-plugin-shortcode-core)
こちらは色を変えるだけでなく，もっと色々できる。こちらは Team Grav謹製。
```
[size=30]大きな字[/size]も書けて，[u]下線も引ける[/u]。
[color=red]これは赤で[/color]，[color=#0000ff]これは青だよ[/color]。
[center]センタリングも[/center]
[right]右寄せも[/right]
[raw][color=red]赤くならない[/color]Raw表示もできる。[/raw]
```
