---
title: "Extended Attributes"
date: 2015-04-22
categories: ["comp"]
tags: ["mac","acl"]
---

`rm` しようとしたらパーミションがないと怒られるファイルやディレクトリがあった。

	$ ls -l
	drwx------+ 2 jun  _lpoperator      68  4 22 18:14 Desktop/
	-rw-r--r--@ 1 jun  _lpoperator  331778  2 16  2010 slide.key

末尾の `+`やら`@`やらは何だろうと`man ls`で調べたらこう書いてあった。

<!--more-->

>  If the file or directory has extended attributes, the permissions field printed by the -l option is followed by a '@' character.  Otherwise, if the file or directory has extended security information (such as an access control list), the permissions field printed by the -l option is followed by a '+' character.

要は, `@`はMac OS Xでの拡張属性が, `+`は拡張セキュリティ情報(ACL等)が設定されていることを示すらしい。
拡張属性の情報は`ls -l@`とか`xattr -l`で表示でき，`man attr`によると特定の属性の削除は

	xattr -d <attr_name> <filename>

全ての属性の削除は

	xattr -c <filename>

でできる。

拡張セキュリティ情報は`setfacl`で変更かと思いきや，Mac OS Xでは`chmod`を使うらしい。ACLsの消去は

	chmod -N <filename>

でできる。

- [Leopard解体新書(4) ～拡張された拡張属性～](http://news.mynavi.jp/column/osx/253/)
- [What is the “+” mark at the end of file description?](http://apple.stackexchange.com/questions/26776/what-is-the-mark-at-the-end-of-file-description)
- [ACLs on Mac OS X](https://www.thomaskeller.biz/blog/2011/06/04/acls-on-mac-os-x/)