---
title: "ShareLaTeX"
date: 2016-12-12
lightbox: true
categories: ["comp"]
tags: ["latex"]
---

オンラインでLaTeX文書の編集・プレビューをできるShareLaTeXを使ってみた。
インターネットさえ使えればLaTeX文書を作れて，共同編集もでき，履歴も残せる。
有料版だと，Dropboxとの同期もできるようで，これまた魅力。
お金を払うか悩み中。。。

<!--more-->

LaTeXのインストールは面倒なことが多いので，数式を書くレポートにLaTeXをちょっと試してみたいというヒトにもお薦め。
[ここ(ShareLaTeX)](https://www.sharelatex.com?r=935fa677&rm=d&rs=b)をクリックしてアカウントを作ってくれると，特典(共同編集者の追加等)があるので嬉しいです。
よく似たのに[OverLeaf](https://www.overleaf.com/signup?ref=c7b4952dbc46)もあって，
どちらが使いやすいかはまだ調査中です。

### 日本語文書の編集方法

日本語文書は，以下のように日本語フォントをアップロードして，XeLaTeXを使うように設定すれば編集できる。

1. フリーの日本語フォントをどこかからダウンロードする。とりあえず[GoogleのNotoフォント](https://www.google.com/get/noto/)の Noto Sans CJK JPにした。
2. ShareLaTeXのファイル一覧にあるメニューでuploadを選択して，以下をアップロードした。
	- NotoSansCJKjp-Regular.ttf
	- NotoSansCJKjp-Light.ttf
	- NotoSansMonoCJKjp-Regular.ttf
3. ShareLaTeXの左上のメニューをクリックして，コンパイラをXeLaTeXに変更
4. 以下のようにフォント設定等をして，文章編集。要は, 日本語文書をShareLaTeXで使うならXeLaTeXでってことになります。

```
\documentclass{article}
\usepackage{xeCJK}
\setCJKmainfont{NotoSansCJKjp-Regular.otf}
\setCJKsansfont{NotoSansCJKjp-Light.otf}
\setCJKmonofont{NotoSansMonoCJKjp-Regular.otf}

\title{桃太郎のお話}
\author{桃太郎}
\date{2016年12月}

\begin{document}

\maketitle

\section{むすび}
ももから生まれた桃太郎が鬼退治をしたというお話でした。
\end{document}
```

Notoフォントには明朝体がないので，設定はとりあえずの適当。
明朝体もある[IPAフォント](http://ipafont.ipa.go.jp/old/ipafont/download.html)あたりをインストールして，mainfontに明朝体，sansfontにゴシック体を割り当てるのが真っ当だろう。

### 情報源
- [ShareLaTeX/Japanese](https://ja.sharelatex.com/learn/Japanese)
