---
title: "SublimeText 3 のプラグイン追加/共有設定"
date: 2015-03-25
categories: ["comp"]
tags: ["sublime text 3"]
---

Sublime Text は軽くてなんだか気持ちよく，「恋に落ちるエディタ」という評判はこういうことかと思うほど．
というわけで，プラグインを少し追加してみた．

<!--more-->

- [Color Highlighter](https://sublime.wbond.net/packages/Color%20Highlighter)

	`#fff` などのカラー指定のテキストをその色で表示できる

- [sublemacspro](https://sublime.wbond.net/packages/sublemacspro)

	emacs風のキーバインド設定

- [OrigamiEmacs](https://sublime.wbond.net/packages/OrigamiEmacs)

	ウィンドウ分割のキーバインドをemacs風に


### 参考URL

- [emacs 使いが、Sublime text 3 を使ってみた](http://blog.fkei.me/2014/05/emacs-sublime-text-3.html)

## Dropboxを使って，違うパソコンで設定共有

	```
	$ cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/
	$ mkdir ~/Dropbox/etc/Sublime
	$ mv User ~/Dropbox/etc/Sublime/
	$ ln -s ~/Dropbox/etc/Sublime/User
	```

新しいマシンにSublime Textをインストールしたときは，上記の設定をしたあと Package Control の設定をすると，必要なパッケージが自動でダウンロードされる.

### 参考URL

- [Package Control -- Syncing](https://packagecontrol.io/docs/syncing)
