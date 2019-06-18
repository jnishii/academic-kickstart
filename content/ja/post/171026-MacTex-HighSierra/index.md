---
title: "MacTeX on High Sierra"
date: 2017-10-27
categories: ["comp"]
tags: ["latex","mac"]
---

科研費の書類をだしたのでMac OSをHigh Sierraにした。
毎年種類の〆切の直前にアップデートしてLaTeXが動かなくなって自爆状態になっていたが，今年は自宅のiMacのアップデートにいろいろトラブルがあったので，申請書提出までは我慢した。

<!--more-->

## High Sierra導入後にしたこと

###  homebrewのメンテナンス
```
$ brew doctor
```
を実行し，出てくる指示に従って，お掃除をした。

### mactexの設定修正

毎度のことながらLaTeXが使えない。。。

[macOS High Sierra + Texlive 2017 + ヒラギノフォント](https://qiita.com/osamu620/items/8a299e44d087542e29a9)を見て以下の設定をした。

1. フォントパス等の設定
```
$ git clone https://github.com/texjporg/cjk-gs-support.git
$ cd cjk-gs-support
$ sudo ./cjk-gs-integrate.pl --link-texmf --fontdef-add=./cjk-gs-integrate-highsierra.dat
$ sudo mktexlsr
```
2. jfontmapsの設定
```
$ git clone https://github.com/texjporg/jfontmaps.git
$ cd jfontmaps
```
Makefile.の1行目を以下に変更
```
TEXMF = /usr/local/texlive/texmf-local.など
```
以下を実行
```
$ sudo make install
$ cd script
$ sudo mktexlsr
$ sudo ./kanji-config-updmap-sys.sh hiragino-highsierra-pron
$ sudo mktexlsr
```

ちなみに，いつもお世話になっている[奥村先生のサイト情報](https://okumuralab.org/bibun7/)によると，以下を実行すると良いらしいが，こちらは試していない。何か不具合があった時に実行する。
```
# ln -sf /System/Library/Fonts/ヒラ*ノ明朝\ ProN.ttc /usr/local/texlive/texmf-local/fonts/truetype/cjk-gs-integrate/HiraginoSerif.ttc
# ln -sf /System/Library/Fonts/ヒラ*ノ丸*\ ProN\ W4.ttc /usr/local/texlive/texmf-local/fonts/truetype/cjk-gs-integrate/HiraginoSansR-W4.ttc
# mktexlsr
```
