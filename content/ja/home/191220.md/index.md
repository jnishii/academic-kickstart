---
title: "Jupyter notebookからPDF/TeX出力"
date: 2019-12-20
categories: ["comp"]
tags: ["jupyter","python"]
---


Jupyter notebook形式で作成したファイルをpdf/TeX形式に変換しようとしたら少しはまったので，今後のためのメモ。

## pdf/TeX形式で出力可能にするための設定

### XeLaTeXのインストール

ubuntu 16.04なら以下の通り。

```
# apt-get install texlive-xetex
# apt-get install texlive-generic-extra
```

### pdf化の日本語対応

`/usr/local/anaconda3/lib/python3.6/site-packages/nbconvert/templates/latex/base.tplx`を修正

```
--- base.tplx.org	2019-12-20 11:30:09.881902845 +0900
+++ base.tplx	2019-12-20 11:32:55.112929136 +0900
@@ -21,6 +21,10 @@
     \else
     	\usepackage{fontspec}
     \fi
+    \usepackage{xeCJK}
+    \setCJKmainfont[BoldFont=IPAexGothic]{IPAexMincho}
+    \setCJKsansfont{IPAexGothic}
+    \setCJKmonofont{IPAGothic}
 
     % Basic figure setup, for now with no caption control since it's done
     % automatically by Pandoc (which extracts ![](path) syntax from Markdown).
```

### 問題点

これで，jupyter notebookのメニューからpdf形式での出力ができるようになり，デフォルトスタイルのままでも結構いい感じなので喜んでいたが，よく見たらMarkdownセルに埋め込んでいた**画像が消えてなくなっている**。いろいろ調べると以下の通りの症状。

- markdownセルにhtmlタグを使って，`<img src="hoge.png" width="600">`などと埋め込んでいた画像は，pdf/texファイルにすると消えてしまう。
- markdownの画像埋め込みタグ`![some image](hoge.png)`で埋め込んだ画像は大丈夫。

画像サイズを調整するにはhtmlタグを使わざるを得ない。
その部分は，適当なスクリプトで画像埋め込みの命令文(`<img src...>`)をLaTeXの命令文(`\includegraphics{...}`)に置き換えるスクリプトで前処理をするくらいしか，今の所解決法が思い浮かばない。
