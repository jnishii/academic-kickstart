---
title: "rsync on Mac"
date: 2016-02-02
categories: ["comp"]
tags: ["mac","unix"]
---

MacBook に rsync でファイル転送をしていたら，`Write failed: Broken pipe`を吐いて頻繁に止まってしまう。
大きめのファイルの転送に時間がかかるのがきっかけのようで，調べてみると接続先の生存チェックを時々するとよいらしい。
<!--more-->

接続元で対応する設定方法は以下のとおり
```
$ cat ~/.ssh/config
ServerAliveInterval 30  # 応答がない時に生存確認まで何秒まつか
ServerAliveCountMax 10  # 生存チェックを最大何回行うか
```

接続先で対応するときは`/etc/ssh/sshd_config`の`ClientAliveInterval`や`ClientAliveCountMax`を同様にいじる。

