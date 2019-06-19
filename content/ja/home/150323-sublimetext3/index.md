---
title: "SublimeText 3をインストールしてみた"
date: 2015-03-23
categories: ["comp"]
tags: ["sublime text 3"]
---

[Atom](http://atom.io/)が重くて残念なことが多いのでSublime Text 3をインストールしてみた。
まだベータ版なので homebrew でインストールするには以下のようにtapが必要。

	$ brew tap caskroom/homebrew-versions
	$ brew cask install sublime-text3

<!--more-->

## Sublime Textを使うための設定

### Package Controlのインストール

1. `Ctrl`-`Shift`-```でコンソールを開く
2. コンソールに[呪文](https://packagecontrol.io/installation#st3)登録

### 日本語化

1. `Cmd`-`Shift`-`P`で Command Pallet を開く
2. Command Palletで`install package`
3. `japanize`を選択
4. 以下を実行
	
	```
	$ cp ~/Library/Application Support/Sublime Text 3/Packages/Japanize/*jp ~/Library/Application Support/Sublime Text 3/Packages/Default/
	```

### Markdownを使うためのパッケージをインストール

- Monokai Extended
	+ markdown の syntax highlight
	+ `Preferences/Color Scheme`で色選択
- Markdown Extended
	+ markdown内のコードのsyntax highlight
	+ `表示/Syntax/Open all with current extension as .../Markdown Extended`を選択
- OmniMarkupPreviewer
	+ L ive Preview   が可能
	+ `⌘`+`⌥`+`O`: Preview Markup in Browser.
	+ `⌘`+`⌥`+`X`: Export Markup as HTML.
	+ `Ctrl`+`Alt`+`C`: Copy Markup as HTML.

### ここまでの参考URL
- [Sublime Text で Markdown を快適にする3つのパッケージ](http://webmem.hatenablog.com/entry/sublime-text-markdown)
- [[Mac] Sublime text 3 インストール はじめにやること - 初期設定 - (Setup 1of5)](http://wp.re13b.jp/entry/sublime/setup/install)

### google 日本語入力で「Tab」キーを有効に

日本語の変換候補を選択するのに「Tab」キーを押すと，候補が消えてなくなるのに対応

1. keymapを保存するdirectoryを作る

	```
	$ mkdir -p ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/Default
	```

2. `Preferences/Key Bindings – Default`を開く
3. (初めての時のみ) `Ctl-s` (保存)して，一回終了して，もう一度2を実行
4. 以下(95-101行目?)をコメントにする

	````
	{ "keys": ["tab"], "command": "insert_best_completion", "args": {"default": "\t", "exact": true} },
	{ "keys": ["tab"], "command": "insert_best_completion", "args": {"default": "\t", "exact": false},
		"context":
		[
			{ "key": "setting.tab_completion", "operator": "equal", "operand": true }
		]
	},
	````


参考URL: [Sublime Text 3 で日本語入力中に Tab キーで入力・変換候補を選択できるようにする](http://beadored.com/sublime-text-3-suggest-tab/)

### 日本語での文字検索をできるようにする

1. 上記の「Tab」キーの設定と同様に，keymapの編集画面にする
2. 以下をコメントアウト

	````
	// Find panel key bindings
	// { "keys": ["enter"], "command": "find_next", "context":
	//  [{"key": "panel", "operand": "find"}, {"key": "panel_has_focus"}]
	// },
	````
3. さらにあと二箇所コメントアウト
	````
	// Replace panel key bindings
	// { "keys": ["enter"], "command": "find_next", "context":
	//  [{"key": "panel", "operand": "replace"}, {"key": "panel_has_focus"}]
	// },
	````
	````
	// Incremental find panel key bindings
	// { "keys": ["enter"], "command": "hide_panel", "context":
	//  [{"key": "panel", "operand": "incremental_find"}, {"key": "panel_has_focus"}]
	// },
	````

参考URL: [Sublime Text 3 で日本語を検索したとき文字が消える不具合を直す](http://memo.sanographix.net/post/101061111635)
