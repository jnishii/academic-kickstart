---
title: "統計検定手法の改革...その4: 検出力"
date: 2016-06-06
categories: ["research"]
tags: ["statistics"]
---

~~検定計算を行う論文を投稿すると，サンプルサイズの妥当性を問われることがあるので~~，
統計計算が必要な研究を行うときには，目的の結論を出すためにサンプルサイズがどの程度必要かを事前に検討する必要がある。

<!--more-->

## 検出力と実験計画

α, βを以下のようにおく

- α: 帰無仮説が正しいのに，それを棄却してしまう確率 (Type I error)
- β: 帰無仮説が正しくないのに，それを正しく棄却できない確率(Type II error)

このとき，1-β (帰無仮説が正しくない時にそれを正しく棄却できる確率)を一般に **検出力**(検定力, Statistical power)とよぶ。
効果量 d を危険率 α, 検出力 1-β の目標値で検出するために必要なサンプルサイズ n を求めることで，実験に必要なサンプルサイズを見積もることができる。
Cohen (1988)は，目標値の目安を `α=0.95, 1−β=0.8`としている。

ただし，**検出力は帰無仮説に基づく検定手法の有効性を示す指標であり，効果量と信頼区間に基づいて二群の比較を行うための指標では(本来は)無い**。
しかし，一般に検定力が高ければ効果量の見積もり精度も高くなるので，後者の場合においても検出力に基づいて実験計画を立てることは妥当とされている
([Nakagawa and Cuthill, 2007](http://www.ncbi.nlm.nih.gov/pubmed/17944619))。

一方で，[Cumming (2014)](http://pss.sagepub.com/content/early/2013/11/07/0956797613504966)は，効果量に注目する場合の検出力に相当する量として計画精度(precision for planning, パラメータ推定精度, accuracy in parameter estimation (AIPE)")を用いるべきと説明している。

## 検出力の計算方法

### G*Power 3

- [G*Power](http://www.gpower.hhu.de/)
	- Windows版とMac版がある(フリー)。GUIで簡単に使える。
- [検定力分析ソフト G*Power について](http://www.relak.net/psy/power/p4.htm)

### Rで計算

- [効果量と検定力 (検出力)を計算するパッケージ　pwr](http://monge.tec.fukuoka-u.ac.jp/R_analysis/effect_size_pwr_01.html)

効果量 d=0.8 を検出するのに必要なサンプルサイズを計算
```
> library (pwr)
> pwr.t.test(d=0.8,power=0.8,sig.level=0.05)
```

## 参考資料

- Cohen, "Statistical power analysis for the behavioral science (2nd ed.)", Routledge, 1998
	- 検定力に関する説明の要点は以下のpdfに書かれている
- [Cohen, "Statical power analysis", Psychological Science, 1(3), 98-101, (1992)](http://courses.csusm.edu/psyc393md/2011-Fall-41892/readings/StatisticalPower/Cohen(1992)StatisticalPower.pdf)
- [効果量・検定力・サンプルサイズ](http://sc1.cc.kochi-u.ac.jp/~murakami/cgi-bin/FSW/fswiki.cgi?page=%B8%FA%B2%CC%CE%CC%A1%A6%B8%A1%C4%EA%CE%CF%A1%A6%A5%B5%A5%F3%A5%D7%A5%EB%A5%B5%A5%A4%A5%BA)
