---
title: "¥ (yen) と \ (backslash)"
date: 2015-09-28
categories: ["comp"]
tags: ["ATOK"]
---

LaTeXの入力には"\" (backslash)が使えないと困るのだけど，Atok for Macのお試し版を使ってみたら代わりに "¥" (yen)が入力されてしまう。
あっさりお試し版を使うのをやめてgoogle日本語入力にすれば環境設定で変更できるのだけど，もう少し使ってみようと思っていろいろ調べたら，以下のファイルを作るのが手っ取り早いらしい。
<!--more-->

```
$ cat ~/Library/KeyBindings/DefaultKeyBinding.dict
{
    "¥" = ("insertText:", "\\");
    "~\\" = ("insertText:", "¥");
}
```
