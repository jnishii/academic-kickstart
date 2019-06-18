---
title: "dvipdfmxのwarning対処"
date: 2014-12-22
categories: ["comp"]
tags: ["latex","mac"]
---

MacTeX の dvipdfmx が謎のwarningを出す。

	`Please consider using "-d" option`

`~/Library/TeXShop/bin/platex2pdf-utf` を修正してdvipdfmxにオプションに

	`dvipdfmx -d 5`

を追加して解決。

<!--more-->

本当は`/usr/local/texlive/2014/texmf-dist/dvipdfmx/dvipdfmx.cfg`の

	`%d 5`

のコメント(`%`)をはずすのが正しい対処法らしいが，これだとMacTeXのバージョンが上がる毎に修正しないといけないので対症療法で対応。
原因はascmacにあるという噂。