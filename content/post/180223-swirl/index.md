---
title: "Rのインタラクティブ教材"
date: 2018-02-23
mathjax: false
categories: ["comp"]
tags: ["R"]
---

R上で動くインタラクティブ教材[swirl](http://swirlstats.com/students.html)の紹介。
以下のように簡単に動きます。

```R
> install.packages("swirl")
> library(swirl)
> swirl()
```

<!--more-->

その後表示されるメニューで以下の2つを選択可能。

```
1: R Programming: The basics of programming in R
2: Regression Models: The basics of regression modeling in R
```
1を選択したら，以下をトピックとして選択可能。
```
1: Basic Building Blocks      2: Workspace and Files
3: Sequences of Numbers       4: Vectors
5: Missing Values             6: Subsetting Vectors
7: Matrices and Data Frames   8: Logic
9: Functions                 10: lapply and sapply
11: vapply and tapply         12: Looking at Data
13: Simulation                14: Dates and Times
15: Base Graphics
```

以下は，"3: Sequences of Numbers"を選択したら出てくる問の一つ。
```
The simplest way to create a sequence of numbers in R is by using the `:` operator.
Type 1:20 to see how it works.
> 1:20
[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
Keep working like that and you'll get there!
```
ちなみに，この部分をまとめてgoogle translateで翻訳すると
```
Rで数列を作る最も簡単な方法は、 `：`演算子を使うことです。 1:20と入力して動作を確認します。
> 1:20
[1] 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
そのように働き続けると、あなたはそこに着くでしょう！
```
設問の方はちゃんとした日本語になっていますね。素晴らしい。
