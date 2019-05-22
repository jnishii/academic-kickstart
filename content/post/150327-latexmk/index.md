---
title: "latexmkの設定"
date: 2015-03-27
categories: ["comp"]
tags: ["latex"]
---

LaTeXを使う時，相互参照や目次作成やらのために何度もlatexやらbibtexやらmendexやらのコマンドを打たないといけないことがある。latexmkを使うと適当に丁度いい回数実行してくれる。
latexmkを使うための設定メモ。

<!--more-->

```
$ cat ~/.latexmkrc
#!/usr/bin/env perl

$latex			= 'platex -synctex=1 -halt-on-error';
$latex_silent   = 'platex -synctex=1 -halt-on-error -interaction=batchmode';
$bibtex         = 'pbibtex';
$biber			= 'biber --bblencoding=utf8 -u -U --output_safechars';
$dvipdf         = 'dvipdfmx %O -o %D %S';
$makeindex      = 'mendex %O -o %D %S';
$max_repeat     = 5;
$pdf_mode       = 3; # generates pdf via dvipdfmx

# Prevent latexmk from removing PDF after typeset.
# This enables Skim to chase the update in PDF automatically.
$pvc_view_file_via_temporary = 0;

# Use Skim as a previewer on Mac OS X
$pdf_previewer    = "open -ga /Applications/Skim.app";
```

内容は必要に応じて修正。

### 参考URL
- [OMake はもう古い！latexmk で快適 TeX コンパイルのススメ](http://konn-san.com/prog/why-not-latexmk.html)
- [Latexmk](http://oku.edu.mie-u.ac.jp/~okumura/texwiki/?Latexmk)