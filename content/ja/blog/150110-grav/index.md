---
title: "Gravのインストール"
date: 2015-01-10
categories: ["comp"]
tags: ["grav"," cms"]
---

サイト構築にしばらくPukiwikiを使っていたが，なんとなく他のCMSを使ってみたくなったので[grav](http://getgrav.org/)を試すことにした．

<!--more-->
### gravの利点
- Markdownを使える
	+ エディタがなんでもOK
	+ 最近書類はMarkdownで書くことが多い
- データベースが不要
	+ バックアップが楽
- インストールが簡単
- 開発がアクティブで，今のところ頻繁にアップデートがある
	+ [Pico](http://picocms.org/)のほうがコンテンツ配置がわかりやすくて良いかと思ったけど，こちらは更新がしばらく止まっている
- ドキュメントがそこそこに整っている
- 数式もMathJaxで表示できる([MathJax](http://getgrav.org/downloads/plugins#)プラグインを利用)

### gravの欠点

- 日本語ドキュメントは殆ど無い
	+ 英語ドキュメントよめばいいので，そんなに問題にはならないけど
- multilingual site に未対応
	+ そのうち対応予定らしい


## インストール

2. [grav](http://getgrav.org/) からzipをダウンロードして展開するだけ
3. 必要に応じて`grav/.htaccess`のRewriteBaseを設定。

## 設定

4. `users/config/site.yaml`をいじる。
5. [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/)を有効にするには`users/config/system.yaml`をいじる。

## リンク

- [ドロップダウンメニュー(antimatter)](https://github.com/getgrav/grav-theme-antimatter)
- [既成のテーマを継承して自分のテーマを作る方法](http://getgrav.org/blog/theme-development-with-inheritance)
