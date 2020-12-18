---
title: "M1 Macの設定"
date: 2020-12-18
categories: ["comp"]
tags: ["homebrew","mac"]
---

Apple Silicon の MacBook Pro の環境構築をしようとしたら，微妙にハマる点があったので作業メモ。

### Homebrew

Homebrewのarm版バイナリが整備されるまでは，arm版とi86版の併用が必要。

[Ressさんのサイト](https://zenn.dev/ress/articles/069baf1c305523dfca3d)を参考に，以下のように設定した。


#### ターミナル

「ターミナルの情報」を開いて，「Rosettaを使用して開く」を有効にする。


#### homebrewのインストール

x86_64版とarm版の両方をインストールする。

1. x86_64版のインストール
    - [Homebrewのサイト](https://brew.sh/index_ja)参照
    - `/usr/local`以下にインストール
2. arm版のインストール。こちらは`/opt/homebrew`以下にインストール
```
$ sudo mkdir /opt/homebrew
$ sudo chown $USER /opt/homebrew
$ curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C /opt/homebrew
```

#### パス設定

1. `.zshrc`に以下を追加。デフォルトではarm版バイナリを探すようにPATH設定をする。

```
setopt no_global_rcs   # ignore /etc/z*
typeset -U path PATH
path=(
    /opt/homebrew/bin(N-/)
    /usr/local/bin(N-/)
    $path
)
```

上記1行目を書かないと，`/etc/z*`で設定しているパスが有効になる。コマンドのPATH探索が思い通りの順と異なって困ることがあるので，今回は自分で設定。

2. brewコマンドのx86_64版とarm版の使い分けのための設定。

```
if [[ "${(L)$( uname -s )}" == darwin ]] && (( $+commands[arch] )); then
    alias brew="arch -arch x86_64 /usr/local/bin/brew"
    alias x64='exec arch -arch x86_64 "$SHELL"'
    alias arm='exec arch -arch arm64e "$SHELL"'
    switch-arch() {
        if  [[ "$(uname -m)" == arm64 ]]; then
            arch=x86_64
        elif [[ "$(uname -m)" == x86_64 ]]; then
            arch=arm64e
        fi
        exec arch -arch $arch "$SHELL"
    }
fi

setopt equals
```
実行例
```
$ arm # ターミナルのarchをarmに
$ x64 # ターミナルのarchをx64に
$ brew update # x64版(Rosetta)
$ =brew update # arm版
$ switch-arch # archの切り替え
```

### Command Line Tools

```
xcode-select --install 
```


### LaTeX

いつもお世話になっている[奥村先生のサイト](https://oku.edu.mie-u.ac.jp/~okumura/macosx/m1.html)を見てインストール。


### その他

Google Backup and Syncでエラーが出たり，F-secureのライセンス認証状況の表示がおかしかったりしたが，何度か再起動したりしているうちに気づいたら正常動作するようになっていた。
