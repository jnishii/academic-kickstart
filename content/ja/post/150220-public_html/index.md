---
title: "public_htmlのパーミション"
date: 2015-02-20
categories: ["comp"]
tags: ["apache", "acl","linux"]
---

`~/public_html`以下に，apache(実行ユーザ名apache)に書き込みを許すときには`setfacl`でパーミション設定をする。
<!--more-->

```
$ setfacl -R -m apache:rwx /home/username/public_html
$ setfacl -R -d -m apache:rwx /home/username/public_html
$ setfacl -R -m username:rwx /home/username/public_html
$ setfacl -R -d -m username:rwx /home/username/public_html
```

`chmod o+x`などで対処する方法もあるが，setfaclのほうがeasy and secure!
