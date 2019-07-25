---
title: "from Grav to Hugo"
date: 2019-07-18
categories: ["comp"]
tags: ["grav","hugo","cms"]
---

このホームページのCMSには[Grav](https://getgrav.org/)を使っていたが，[Hugo](https://gohugo.io/)に変えた。
Gravラブだったが，セキュリティ確保のための学内方針で静的サイトに移行しないといけないため，止むを得ずというところ。

<!--more-->

Gravは使いやすいように非常によく設計されており，不具合も少なく，カスタマイズもしやすい。
これまでに少しずつテーマのカスタマイズもしてきたので，Gravで生成したサイトを静的サイトに変換する方法も検討したが，[blackhole](https://github.com/BarryMode/grav-plugin-blackhole)はblogページのページ分けができず，httrackでのダンプも試したがやたら時間がかかる。

Hugoを選択した理由は以下の通り。

1. Markdownで書き溜めたGravのコンテンツを，少しヘッダを修正すれば使える
2. Hugoのテーマ[Academic](https://themes.gohugo.io/academic/)が割とよく出来ていて，gravからの移行もしやすそうだった。
3. Gravのテーマ[learn2](https://github.com/getgrav/grav-theme-learn2)も使っていたが，Hugoにはこれのレプリカ版のテーマ[Learn](https://themes.gohugo.io/hugo-theme-learn/)があったので，こちらのコンテンツの移行も楽そうだった。

Hugoのテーマいじりにはあまり時間をかけないように...と思っていたが，Gravで簡単に使えた機能がHugoではすぐにできないことがいろいろあったのが残念で，結局テーマの修正にも時間がかかり，少しずつ修正を続けている。以下は困った点(もしくは，困っている点)。

1. mathjaxで処理するlatexコマンドに`_`や`*`など，Markdownの書式設定用の記号が入っているとmathjaxの処理がされない。例えば`a_{12}`や`\begin{align*}`はだめ。`a\_{12}`としたり，displayMathなら`<div>..</div>`で囲ったりする。[詳細はこちら](https://gohugo.io/content-management/formats/#issues-with-markdown)。(注: academicテーマでは，[ver4.4](https://sourcethemes.com/academic/updates/v4.4.0/)からこの問題がなくなったらしい。)
2. baseURLに`http://baseurl/~someone/`のようにディレクトリ名が入っていると，baseURLの指定をしてもメニュー等のリンクがおかしくなることがある。特に多言語サイトにしたとき，`{{ .URL | relLangURL }}`などと`relLangURL`でURLの変換をすると，なぜか`/~someone/`がbaseURLに追加されて`http://baseurl/~someone/~someone/`となる。
["このページ"](https://discourse.gohugo.io/t/issue-when-baseurl-have-subdirectory/4088)には，
```
RelativeURLs=true
CanonifyURLs=true
```
の指定で大丈夫とあるが，`relLanURL`の振る舞いは変わらないのでこちらは不採用。試行錯誤で見つけた方法は以下のとおり。
    - メニュー項目の`url`指定の頭には`/`はつけない。([参考1](https://github.com/pacollins/hugo-future-imperfect-slim/issues/34),[参考2](https://github.com/gohugoio/hugo/issues/6073))
    - baseURLには`/~someone/`のみを指定
3. Gravでは任意のフォルダのコンテンツをトップページに指定でき，コンテンツのフォーマットの変更も簡単。Hugoはコンテンツをおく位置や名称で，そのフォーマットや役割(トップページとかblogページとか)を決めるのが原則なので，それをちょっと変えようとすると結構面倒。例えば，academicテーマの場合，トップページのコンテンツは`content/home/`, blog用のページは`content/post/`に決め打ちで，これを変更しようとすると四苦八苦する。[関連情報はこちら](https://github.com/gcushen/hugo-academic/issues/356)。
4. Gravでは良きにはからってくれることが，Hugoではなかなかうまくいかないことが多いが，Hugoが使い勝手が悪いというよりは，Gravの設計が非常によく出来ているという印象。



