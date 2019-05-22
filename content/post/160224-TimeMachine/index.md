---
title: "TimeMachine on Mac"
date: 2016-02-24
categories: ["comp"]
tags: ["mac"]
---

デスクトップ機として使っている iMac (27inch, Late 2009)がよく熱暴走する。
とりえず動いている時にもレスポンスが遅いので，気づくと画面の小さなノートパソコンで作業することが多くなってきてデスクトップ機がある意味がない。
これは勿体ないとハードディスクをSSDに換装した。
感動的に速く，そして静かになった。

<!--more-->

ただ，困ったことにこれまで使っていたTimeMachineにバックアップを取れなくなった。
これまでのバックアップを無駄にするのは怖いので，TimeMachineの中のログファイルを見ると，
`Alfred 2`のバックアップでエラーを出している。

```
Fatal Error: (-43) SrcErr:NO Copying /Applications/Alfred 2.app to /Volumes/TM/Backups.backupdb/dolce/2016-02-23-123606.inProgress/FA092B97-2246-49F7-80EA-6E8D6BA93AFA/Mac SDD/Applications
```

`Alfred 2`を削除してTimeMachineを再起動したらめでたく動いた。
`Alfred 2`もその後に再インストールしてちゃんと動いている。
SDDへの換装後にTime Machineのデータを流し込んだのだけど，Time Machineを動かす前に`Alfred 2`を再インストールしたのがどうもまずかったらしい。

それはともかくTime Machineからのデータ移行時の選択肢に，`その他のフォルダやファイル`として130GBほど表示された。悩んだ挙句移行しなかったのだけど，これは一体何のファイルだったんだろう...

