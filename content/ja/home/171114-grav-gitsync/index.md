---
title: "gravのgitsync対応"
date: 2017-11-14
categories: ["comp"]
tags: ["cms","grav"]
---

これまでDropbox管理だった[Grav](http://www.grav.org)を[GitHub](http://github.com)対応にして[git-syncのプラグイン](https://github.com/trilbymedia/grav-plugin-git-sync)を使おうとしたらはまった。
一応なんとかなったと思うので，今後のためのメモ書き。

<!--more-->


### したかったこと

これまで[Grav](http://www.grav.org)(CMS)のファイルはDropboxで同期してコンテンツ更新をして，最後にrsyncでwebサーバに反映していた。
Dropboxだと変なコンフリクトが起きることがあって微妙に嫌なことがあるのと，rsyncが面倒(コマンド一発だけど，いろいろな事情でwebサーバにアクセル出来る場所が限られる)。
Dropboxはやめて，すっきりと[GitHub](http://github.com)管理にして，[git-syncのプラグイン](https://github.com/trilbymedia/grav-plugin-git-sync)でwebサーバに自動反映しようとしたらはまった。

### したこと

1. gravのディレクトリ全体をGitHubで管理しようとレポジトリを作成。
2. ローカルレポジトリをGitHubに反映し，webサーバ上で`git clone`を実行してダウンロード
2. webサーバ上で，[git-syncのプラグイン](https://github.com/trilbymedia/grav-plugin-git-sync)を導入して設定。
	1. admin パネルを導入
	2. pluginメニューで設定。表示の通りやれば設定は簡単。GitHubで2FAを使っていてもaccess tokenで設定できるように誘導してくれる。
4. 一通り設定した後，GitHubに反映させようと`git push`を実行

### 困ったこと

1. ローカルレポジトリで`git pull`をした後，動作確認のためコンテンツ(`user/pages/`以下)をいじって`git push`するが，サーバファイルに自動反映されない。
2. webサーバサーバ上で`git pull`しても，やはりダメ。`user/pages/`以下のファイルは更新されているので，ますます意味がわからず悩む。


### 原因

なぜかローカルレポジトリを見ていたら，`user/pages/`に配置されているディレクトリ`pages`が，トップディレクトリ**にも**できている。ただし，こちらには上記の変更が反映されていない。
これで，git-syncプラグインは`pages`コンテンツのみをレポジトリ登録することが判明。つまり，gravツリー全体を管理するように作ったレポジトリに，さらにgit-syncプラグインがpagesのみをそこに登録(`user/`をトップディレクトリとして，`pages`以外は無視して登録)して，わけのわからん状態になっていた。

### 解決方法

素直に，gravツリーとpages以下を，別のレポジトリに分けることにした。

1. gravツリーのレポジトリを作る(`pages`以下は削除したもの。レポジトリ`grav`とする)
	- `.gitignore`には以下を追加
		```
		user/pages
		```
2. `pages`コンテンツ用の**空のリポジトリ**(`grav-pages`とする)もGitHubに準備
3. webサーバ上ではレポジトリ`grav`をダウンロードしたものに`pages`コンテンツを加えた，ツリー完成版を作る。 ここでgit-syncプラグインの設定して，レポジトリ`grav-pages`を登録
4. 管理用パソコンで，`git clone <grav-pagesのレポジトリ>`をして，コンテンツが登録されていることを確認。
5. `grav`レポジトリの`usr/`以下から，上記`<grav-pagesレポジトリ>/pages`へのシンボリックリンクを作る

### 感想

- 手元でコンテンツを更新して`git push`をしたらすぐにwebページに反映される。なんか気持ちいい。バックアップをとる心配もなくなって満足。


### 残った問題と解決方法 

- gravツリーのwebサーバへの反映は自動では出来ない。これは頻度も少ないのでwebサーバ上で`git pull`することにする。

### おまけ

こんなことで延々と時間を潰して何やってんだか。。。
GitHubの webhookのしくみとか，access token のこと等いろいろとわかったこともあったので，そこそこに収穫もあったけどね。
