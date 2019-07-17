---
title: "brew-caskでインストールしたアプリをAlfredで呼び出す方法"
date: 2015-03-26
categories: ["comp"]
tags: ["homebrew", "alfred"," mac"]
---

brew-caskでインストールしたアプリが[Alfred](http://www.alfredapp.com/)で呼び出せないのでgoogle先生に聞いたら，以下のようにすればよいとのこと．

<!--more-->
	```
	$ echo "export HOMEBREW_CASK_OPTS=\"--appdir=/Applications\"" >> ~/.bash_profile
	$ . ~/.bash_profile
	$ brew cask install alfred
	$ brew cask alfred link
	```

Alfredの`Preferences/Features`の`Search Scape`に`/opt/homebrew-cask/Caskroom/`を追加してもOK...というか，こちらが簡単かも.

### 参考URL
- [homebrew-caskでインストールしたアプリをalfredから呼び出すときのメモ](http://qiita.com/kametaro/items/942cafd910fc7989be3a)
- [brew cask alfred linkが失敗するときの対処](http://qiita.com/u1tnk/items/98175c5f8377f01c3da9)