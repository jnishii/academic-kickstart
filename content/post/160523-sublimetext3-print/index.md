---
title: "SublimeText 3で印刷"
date: 2016-05-23
categories: ["comp"]
tags: ["sublime text 3"]
---

Sublime Text 3で編集中のプログラムを印刷しようとして，印刷コマンドがないことに気づいた。
印刷できるようにするにはパッケージ追加が必要。

<!--more-->

## Sublime Text 3の印刷設定


### [ExportHtml](https://github.com/facelessuser/ExportHtml)を使う

1. `Cmd`-`Shift`-`P`で Command Pallet を開く
2. Command Palletで`install package`
3. "[ExportHtml](https://github.com/facelessuser/ExportHtml)"を選択
4. `Key Bindings - User`に以下を設定
```
{
    "keys": ["ctrl+alt+n"],
    "command": "export_html",
    "args": {
        "numbers": true,
        "wrap": 900,
        "browser_print": true,
        "multi_select": true,
        "color_scheme": "Packages/ExportHtml/ColorSchemes/Print-Color.tmTheme",
        "style_gutter": false
    }
}
```
