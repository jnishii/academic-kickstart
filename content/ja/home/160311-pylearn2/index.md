---
title: "ディープラーニングのシミュレーション環境を Mac に構築"
date: 2016-03-11
mathjax: false
categories: ["comp"]
tags: ["python"," mac"]
---

AnacondaとPylearn2のインストール方法を卒論生に教わった。
こういう卒論生が増えてくれると頼もしい。
でも，Macにインストールしようとしたら色々と苦労したので，インストール方法を忘れないうちにメモ。

<!--more-->

ちなみに google で pylearn2 を引っ掛けると山のように検索結果が出てくる。
「ちょっとディープラーニングでも」と遊べる環境がすぐ整うのは素晴らしい。
もっともディープラーニングのシミュレーションをしたいのではなく，小規模な神経回路網モデルのシミュレーションで遊びたいだけなんだけど。				

- [Anaconda](https://www.continuum.io/why-anaconda)
	- pythonとデータ解析モジュールのセット
- [Pylearn2](http://deeplearning.net/software/pylearn2/)
	- 言わずもがななディープラーニングのモジュール

## Anacondaのインストール

### 準備: pyenv のインストール
[pyenv](https://github.com/yyuu/pyenv)は，複数のバージョンのpythonをインストールしたり，使うバージョンを切り替えたりするのに便利なツール。

1. pyenvのインストール
```
$ brew install pyenv
```
2. pyenvの環境設定:
`pyenv`でインストールするAnaconda等は，デフォルトでは`~/.pyenv/shims`にインストールされるが，代わりに，homebrew のディレクトリツリーにインストールしたい時は，以下を`~/.bash_profile`に追加する。
```
export PYENV_ROOT=/usr/local/var/pyenv
```
3. パス設定: shimsにパスを通して，コマンド補完もできように以下も`~/.bash_profile`に追加する。
```
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```
最後に`~/.bash_profile`の読み込みをお忘れなく。
```
$ . ~/.bash_profile
```

### Anacondaをインストール
```
$ pyenv install anaconda3-2.5.0	# anaconda3-2.5.0 の python  は version 3.5.1
$ pyenv rehash
$ pyenv global anaconda3-2.5.0	# anaconda3-2.5.0をデフォルトに設定
$ conda update conda			# パッケージのアップデート
```

pythonのモジュールは一般に`pip`コマンドで管理するが，anaconda 環境の python モジュールは`conda`で管理する。(`conda` の使い方は下の方にメモ書きあり。)

無事インストールできたかは以下のように確認できる。
```
$ pyenv versions
  system
* anaconda3-2.5.0 (set by /Users/jun/.pyenv/version)
$ which python
/Users/jun/.pyenv/shims/python
$ python --version
Python 3.5.1 :: Anaconda 2.5.0 (x86_64)
```

## Pylearn2のインストール

### 準備: Theanoのインストール

[Theano](http://deeplearning.net/software/theano/)は行列計算等を高速二身とか地理り行うモジュール。pylearn2を使うには開発版をインストールする必要があるらしい。`conda` ではモジュールが無いみたい(Linux用はあるみたいだが)なので，[Theanoのインストール方法](http://deeplearning.net/software/theano/install.html#anaconda)の指示に従って，`pip`コマンドでインストール。

```
$ pip install --upgrade --no-deps git+git://github.com/Theano/Theano.git
```

### Pylearn2のインストール

Pylearn2はローカルディレクトリにダウンロード・インストールする。
`~/python`以下にインストールするなら，以下のようにする
```
$ cd ~/python
$ git clone git://github.com/lisa-lab/pylearn2.git
$ cd pylearn2
$ python setup.py build
$ python setup.py install
```

### パスの設定
```
$ cat .bash_profile
export PYTHONPATH=$PYTHONPATH:${HOME}/python/pylearn2
```

## 遊び方

あとはgoogle先生に教わって，思う存分遊んでください。


## おまけ: conda の使い方
`conda` は anaconda に入っている python のモジュールを管理するツール。

- `conda info -e` : デフォルトの python の環境を表示
- `conda list` : インストールされているモジュール一覧
- `conda search <module>` : 利用できるモジュールのバージョン情報
- `conda install <module>` : モジュールのインストール
- `conda update conda` : conda コマンドの更新
