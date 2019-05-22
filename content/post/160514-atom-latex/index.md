---
title: "AtomにLaTeX関連のパッケージを追加"
date: 2016-05-14
categories: ["comp"]
tags: ["atom"]
---

[Atom](http://atom.io)にLaTeX関連のパッケージを追加した。

- [LaTeX Package](https://atom.io/packages/latex)
  - `ctl-alt-b`でlatexコマンド実行
  - 注意) latexmk がインストールされていること
- [PDF View package](https://github.com/izuzak/atom-pdf-view)
  - 編集中のLaTeXファイルの出力PDFを開いておけば，PDFファイルが更新される毎に表示も更新される。
  - 表示されているPDFファイルをクリックすると，**LaTeXファイルの対応箇所に飛ぶことができる**。すばらしい\(^o^)/

<!--more-->

```
$ apm install latex
$ apm install pdf-view
```
