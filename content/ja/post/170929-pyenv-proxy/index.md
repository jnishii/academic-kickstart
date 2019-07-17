---
title: "pyenvでproxy超え"
date: 2017-09-29
lightbox: true
categories: ["comp"]
tags: ["python"]
---

pyenvでanacondaをインストールしようとしたが，以下のようにエラーが出る。
<!--more-->

```
$ pyenv install anaconda3-4.4.0
Downloading Anaconda3-4.4.0-Linux-x86_64.sh...
-> https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
error: failed to download Anaconda3-4.4.0-Linux-x86_64.sh

BUILD FAILED (Vine 6.5 using python-build 1.1.3-27-g8297de9)
```


学外にアクセスするためにproxy設定は，以下のように設定済み。

```
$ cat ~/.bash_profile
...
export http_proxy="http://proxy.hoge:port"
export https_proxy="https://proxy.hoge:port"
export ftp_proxy="ftp://proxy.hoge:port"
```
pyenvがダウンロードに使うcurlを使って，直接ダウンロードすると
```
$ curl https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
curl: (7) Unsupported proxy scheme for 'https://proxy.hoge:port
```
とでる。ちなみに，curlのバージョンは
```
$ curl -V
curl 7.51.0 ...
```
proxy が httpsの場合，curlのバージョンは7.52以上でないとダメらしい[情報源](https://daniel.haxx.se/blog/2016/11/26/https-proxy-with-curl/)。
curlをupgradeすればよいが，とりあえずhttps proxyを偽造したら動いた。
```
$ https_proxy=http://proxy.hoge:port pyenv install anaconda3-4.4.0
Downloading Anaconda3-4.4.0-Linux-x86_64.sh...
```

ちなみに，curl単体で使う時のproxy設定は`~/.curlrc`に以下のように書けば良い。
```
$ cat ~/.curlrc
proxy="http://proxy.hoge:port"
```
しかし，pyenvがcurlを呼び出す時にはhttpsのほうを押し付けられて，上記のような困ったことになるみたい。
