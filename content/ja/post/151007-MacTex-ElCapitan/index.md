---
title: "MacTeX on El Capitan"
date: 2015-10-07
summary: El CapitanにMacTeXを導入した...
categories: ["comp"]
tags: ["latex","mac"]
---

誘惑に負けてMac OSを最新版のEl Capitanにしてしまった。
Mac OSの新バージョンが出るのはいつも今頃の新学期の準備やら科研費やらのシーズンで，以前うっかりアップデートしてTeXが動かなくなって慌てることがあった。
今回はしばらく様子を見てからと思いながら，Macからのお知らせにつられて，ついプシッとアップデートのクリックをしてしまった。で，やっぱりTeXまわりで困ってしまった。

### El Capitan導入後にしたこと

1. homebrewのメンテナンス
```
$ brew doctor
```
を実行し，出てくる指示に従って，お掃除をした。

2. mactexの設定修正

いつもお世話になっている[奥村先生のサイト情報](https://oku.edu.mie-u.ac.jp/~okumura/texwiki/?TeX%20Live%2FMac#elcapitan)にしたがって，以下を実行した。
```
$ sudo tlmgr update --self —all
$ cd /usr/local/texlive/2015/texmf-dist/scripts/cjk-gs-integrate
$ sudo perl cjk-gs-integrate.pl --link-texmf --force
$ sudo mktexlsr
$ sudo updmap-sys --setoption kanjiEmbed hiragino-elcapitan-pron  
```
最後の行はヒラギノフォントのNシリーズ(ProN/StdN)を使う場合。Nシリーズでない(Pro/Std)ほうを使うこともできるが，[ヒラギノフォントの販売元](http://www.screen.co.jp/ga_product/sento/support/otf_osx_El_Capitan.html)によるとEl Capitan搭載のヒラギノフォント(Pro/Std)には互換性のための深刻な問題があるので使用しないようにとのこと。

### 困った症状と対応

dvipdfmx が以下のエラーを出すようになった。

```
Running 'dvipdfmx  -"hoge.pdf" "hoge.dvi"'
hoge.dvi -> hoge.pdf
[1
dvipdfmx:warning: Could not locate a virtual/physical font for TFM "rml".
dvipdfmx:warning: >> This font is mapped to a physical font "HiraginoSerif-W3.ttc".
dvipdfmx:warning: >> Please check if kpathsea library can find this font: HiraginoSerif-W3.ttc
dvipdfmx:fatal: Cannot proceed without .vf or "physical" font for PDF output...
```
日本語フォント(`HiraginoSerif-W3`)の設定が悪いとのことなので，心当たりのディレクトリをいろいろ見てみたら，以前のフォント設定の残骸が悪さをしているみたい。個別に修正するよりは再インストールが早いと思い，以下を実行した。

```
$ brew cask uninstall mactex
$ sudo rm -rf /usr/local/texlive/             <= 残骸の処理
$ sudo rm -rf /usr/local/share/ghostscript/   <= 残骸の処理
$ brew cask install mactex
```
その後，再び上記のmactexの設定も行った。

### 困った症状とその対策(その２)

またまた，dvipdfmxがエラーを吐く。

```
Running 'dvipdfmx  -o "hoge.pdf" "hoge.dvi"'
------------
hoge.dvi -> hoge.pdf
[1][2][3Error: /typecheck in /findfont
Operand stack:
   GothicBBB-Medium-83pv-RKSJ-H
Execution stack:
```

`GothicBBB-Medium-83pv-RKSJ-H` なんてフォントは使ってないぞと思いながら調べたら，結構前からあるmatlabの問題で，matlabで作ったepsにこのフォント設定が入り込むことがあるらしい。

- [参考サイト:「MATLABで作成したEPSファイルに含まれる日本語フォントを削除する(2007-05-21) 」](http://hinf.ee.utsunomiya-u.ac.jp/~hirata/pukiwiki/?MatlabTips)

`GothicBBB-Medium-83pv-RKSJ-H`を適当に処理するように設定するのも一手だけど，英文タイトルのフォント指定にこういう日本語ローカルなフォントが入り込むのはトラブルのもとなので，余計な設定はしないほうが良さそう。
上記サイトにあるように，epsファイルの上記フォント設定を英文フォント(Helvetica)に置換したら無事に`dvipdfmx`を使えるようになった。

めでたしめでたし。


