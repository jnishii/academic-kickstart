---
title: "Julia"
date: 2015-06-15
categories: ["comp"]
tags: ["julia"]
---

[julia のページ](http://julialang.org/)に, いろいろな科学計算用のプログラミング言語の計算速度比較の表がある。
matlab や octave は Toolbox が豊富なのでコーディングが楽なことが多いけど，重い計算には向かないのがよくわかる。
案外がんばっているのは mathematica. 
<!--more-->

julia は[開発動機](http://julialang.org/blog/2012/02/why-we-created-julia/)に

> We want the speed of C with the dynamism of Ruby.

とあるだけに高速度でフレキシブル。パッケージも増えてきているし文法も平易なので，ちょっとしたプログラムは julia で組んで，計算速度がかかるルーチンがあったらそこだけ C にして julia から呼び出したり，分散処理にしたりというのが良さそう。
[講義や演習に導入している大学](http://julialang.org/teaching/)も増えているらしい。

もっとも，計算速度はさておいてとりあえずデータ処理をというだけなら，お手軽な言語は他にも山のようにあるし，挙げ句の果てには巨大合体ロボットみたいな [SageMath](http://www.sagemath.org/) なんかもあって，良い時代。
一番大事なのは結局何を知りたいかという動機だけど，そんなことはさておいて技術を闇雲に蓄えておくといざというときの底力になる。

というわけで，みんなで楽しく勉強しましょ。


### Julia 関連のリンクの覚え書き

- [Juliaのインストール、パッケージ追加、簡単なplotまで](http://www.mwsoft.jp/programming/julia/install_plot.html)
- [Why We Created Julia](http://julialang.org/blog/2012/02/why-we-created-julia/)
