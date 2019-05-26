---
title: "R/tidyverse"
date: 2018-08-01
mathjax: false
categories: ["comp"]
tags: ["R"]
---

同じデータを手にしても，それを表にまとめるときには，ある人は列に並べる値を，別の人は行に並べたりする。ある人が一つの表にまとめるようなデータを，別の人は複数の表に分けてまとめることもある。
というわけで，データ処理のときに案外大変なのが，手にしたデータのフォーマットを解析しやすいフォーマットに変換する前処理だったりする。

<!--more-->

Rの[tidyverse](https://www.tidyverse.org/)パッケージ群では，データの標準形式(tidy data, 整然データ)を定めて，入手したデータを標準形式に変換したり，加工したり，表示したりするためのツールを提供している。
これと[magrittr](https://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html)パッケージのパイプ処理を組み合わせると，実にすっきりとデータ処理をスクリプト化できる。例えば，気温データファイル`data`に，`year`, `month`, `day`, `temperature`  のデータがあるとき，
月ごとの平均気温を計算するのは以下でできる。
```
data %>% group_by(year,month) %>%
	summarise(mean(temperature))
```

もはや，Rというよりも，tidyverseという言語を使うとRの機能も使えるよというほうが良い気がするくらいRのプログラミング方法が変わる。
データサイエンス系のプログラム言語はRよりもPythonが主流になっていくかなと思っていたが，googleで検索してみると，tidyverse があるからと R を使う人も多いみたい。

tidyverseに興味がある人には，[R for Data Scence](http://r4ds.had.co.nz/)が入門におすすめ。
日本語訳も出版されているけど，英語版なら無料でオンラインで見れる。
ついでながら，メモ書きページにも[tidyverseのメモ](http://bcl.sci.yamaguchi-u.ac.jp/~jun/notebook/r/tidyverse)を時々書いている。





