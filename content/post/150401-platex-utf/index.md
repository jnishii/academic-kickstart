---
title: "pLaTeXで外字を使う方法"
date: 2015-04-01
categories: ["comp"]
tags: ["latex"]
---

LaTeXで旧字体の人名を表示できなかったので設定方法を調べた．わりと簡単．

	\usepackage{otf}

とotfパッケージを読み込む．使いたい漢字はUTFコードで指定する．
UTFコードは，Mac OS Xなら例えばgoogle 日本語入力メニューの「文字パレット」や「手書き文字入力」で調べることが出来る．
入力は例えは`\UTF{FA11}`と指定する．

そもそも書類を書くのはPagesゃWordなどを使えばこんなことに悩まなくてすむのだけど，
最近はmarkdownでいろいろ書くことが多く，これだと軽いエディタで編集できるし, 
ちょっと読むときは lv ですむのが快適でなかなか抜け出せない．
たまに配布用に pandoc で LaTeX 形式にしたときにこういう困った事態がたまにおきるが，
それでもワープロソフトを使うのに比べるとストレスは少ない気がする．

### 参考URL

- [Mac OS X 上のLaTeXで外字、異体字を使う方法](http://osksn2.hep.sci.osaka-u.ac.jp/~taku/osx/gaiji.html)