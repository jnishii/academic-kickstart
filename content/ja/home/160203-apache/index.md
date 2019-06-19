---
title: "Apache on Mac"
date: 2016-02-03
categories: ["comp"]
tags: ["mac","unix","apache"]
---

Mac での Apache の設定・起動方法をときどき忘れるのでメモ。

西井はディレクトリ`~/Dropbox/Sites`にCMS([grav](https://getgrav.org/))を入れて，これに`http://localhost/~jun`でアクセスできるようにしている。

1. apache の設定ファイル`/etc/apache2/httpd.conf`の設定
  1. 以下の各命令文の行頭の`#`をとってコメントアウトを中止。
  ```
  LoadModule userdir_module libexec/apache2/mod_userdir.so
  LoadModule rewrite_module libexec/apache2/mod_rewrite.so
  LoadModule php5_module libexec/apache2/libphp5.so
  Include /private/etc/apache2/extra/httpd-userdir.conf
  ```
  2. 起動ユーザ名を自分のログイン名にした。つまり
  ```
  User _www
  Group _www
  ```
  とあったのを以下に変更。
  ```
  User jun
  Group staff
  ```
2. `/private/etc/apache2/extra/httpd-userdir.conf`を修正
  1. `UserDir`を修正
  ```
  UserDir Dropbox/Sites
  ```
  2. 以下の命令文の行頭の`#`をとってコメントアウトを中止。
  ```
  Include /private/etc/apache2/users/*.conf
  ```
3. `/etc/apache2/users/jun.conf`を追加
```
$ cat /etc/apache2/users/jun.conf
<Directory "/Users/jun/Dropbox/Sites/">
    Options Indexes FollowSymLinks Multiviews
    RewriteEngine on
    AllowOverride All
    Require all granted
</Directory>
```

apacheの起動等のコマンドは以下のとおり。
```
$ sudo apachectl configtest # 設定ファイルのテスト
$ sudo apachectl restart    # apache起動
```

ブート時に自動起動する方法は以下のとおり。
```
$ sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```
