---
title: "SublimeText 3にLaTeXプラグイン追加"
date: 2016-06-04
categories: ["comp"]
tags: ["sublime text 3"]
---

Sublime Text 3にLaTeX文書作成用プラグイン[LatexTools](https://github.com/SublimeText/LaTeXTools)をMacに入れた。

<!--more-->

1. `Cmd-Shift-P`でCommand Pallette起動
2. Package Control: Install Package 起動
3. LatexToolsをインストール
4. 設定ファイル
```
~/Library/Application\ Support/Sublime\ Text\ 3/Packages/LaTeXTools/builders/traditionalBuilder.py
```
の`DEFAULT_COMMAND_LATEXMK`の設定を以下に変える。(普段使っているlatexmkの設定に応じて修正)
```
DEFAULT_COMMAND_LATEXMK = ["latexmk", "-cd",
"-e", "$latex = 'platex %O -interaction=nonstopmode -synctex=1 %S'",
"-e", "$biber = 'biber %O --bblencoding=utf8 -u -U --output_safechars %B'",
"-e", "$bibtex = 'upbibtex %O %B'",
"-e", "$makeindex = 'mendex -U -r -c -g -s ~/Dropbox/etc/latex/dot.ist %O -o %D %S'",
"-e", "$dvipdf = 'dvipdfmx %O -o %D %S'",
"-f", "-norc", "-gg", "-pdfdvi"]
```

`Cmd-b`でlatexmkコマンドを起動できる。

注) MacTeXとSkimがインストールしてあること。

参考URL
- [OS X El Capitan＋Sublime Text 3でヒラギノを埋め込んだPDFを作る
Raw](https://gist.github.com/wichy/13c970563a2f5a489c9d)
- [TeX Wiki Sublime Text](https://texwiki.texjp.org/?Sublime%20Text#c2e22368)