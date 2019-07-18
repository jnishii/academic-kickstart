---
title: "Gravの覚書"
date: 2015-08-31
categories: ["comp"]
tags: ["grav"," cms"]
---

夏バテなのか研究室のWebサーバのハードディスクが壊れてしまった。
このWebページの復旧作業にもえらく手間取ってしまったので作業メモ。

<!--more-->

### grav のプラグインをインストールできなくなった...
```
$ bin/gpm install archives
Preparing to install Archives [v1.3.0]
  |- Downloading package...   100%
  |- Checking destination...  ok
  |- Installing package...  PHP Fatal error:  Class 'ZipArchive' not found in /home/jun/public_html/grav2/system/src/Grav/Common/GPM/Installer.php on line 73
Whoops\Exception\ErrorException: Class 'ZipArchive' not found in file /home/jun/public_html/grav2/system/src/Grav/Common/GPM/Installer.php on line 73
Stack trace:
  1. () /home/jun/public_html/grav2/system/src/Grav/Common/GPM/Installer.php:73
```
あれれと思って調べたら，php に zip モジュールが追加されていなかった。

```
# pecl install zip
You should add "extension=zip.so" to php.ini
```
というわけで，指示に従って`php.ini` も修正した。

### 各ページヘッダ部の画像が「Error」と真っ赤に表示される。

ヘッダ部の大きな画像は**大きく「Error」と真っ赤に表示**されるので，まさに**炎上**という気分。

なかなか原因がわからなかったが，元画像と同じディレクトリにあった隠しファイル(元がpic.jpg なら 隠しファイル名は `._pic_jpg`が作られていることが問題だったらしい。
`bin/grav --clear-cache`でも消去されないようなので，手動でこのファイルを全部消したところ，無事に画像を表示できた。
```
$ cd grav/usr/pages
$ find ./ -name '._*' | xargs rm
```
どのタイミングでこのファイルが出来てしまったかは謎。
