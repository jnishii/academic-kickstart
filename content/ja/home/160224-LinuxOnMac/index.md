---
title: "Linux on Mac"
date: 2016-02-24
categories: ["comp"]
tags: ["mac"," linux"]
---

机の上にある Mac mini (Mid 2007) がオブジェ化していたので，再利用のために Vine Linux 6.3をインストールした。
手順の概要は[ここ](http://trac.vinelinux.org/wiki/Developers/inagaki)とほぼ同じだけど，少し違うところがあったのでメモ。
ただし，CPUパワーとメモリにゆとりがある現役バリバリマシンに Linux を入れるなら[VirtualBox](https://www.virtualbox.org/) や [VMware Fusion](http://www.vmware.com/jp/products/fusion/) で仮想化した方がてっとり早く安全です。

<!--more-->

## 準備

### Linux は32bit版と64bit版のどちらを使えるかチェック

古いMacの場合，64bit OSは動かない。CPUが Core Duo 以前なら 32bit 版のLinuxをインストール。西井のMac mini (Mid 2007)はCore 2 Duoだけどアウト。
確認方法は以下の通り。
- [ここ](https://support.apple.com/ja-jp/HT3770)か
[ここ](https://support.apple.com/ja-jp/HT4287)を見て Mac OS Xの64bitカーネルが動いているかどうか確認。
	- 64bitカーネルが動いていたら Linux も 64bit 版でOK。
	- 64bitカーネルが動いていなかったら諦めて 32bit 版を使う。ただし，
```
$ ioreg -l -p IODeviceTree | grep firmware-abi
```
の結果
```
    | |   "firmware-abi" = <"EFI64">
```
と表示されたら，EFI(Macのブートシステム)は64bit対応なので頑張れば64bit版をインストールできる(方法はgoogle先生に聞いてください)。
西井のMad mini (2007 Mid)のように，CPUがCore 2 DuoなのにEFIが32bit版の時も
[ここ](http://ubuntuforums.org/showthread.php?t=2287767)や[ここ](http://www.rodsbooks.com/ubuntu-efi/)や
[ここ](http://blog.christophersmart.com/2009/12/22/updated-efi-grub2-tarball-including-64bit/)や
[ここ](http://forum.netkas.org/index.php/topic,1123.0.html)
を見て頑張れば 64bit版Linuxをイントールできるようですが少々面倒そうなので試してません。


## インストール

1. Mac OS Xのディスクユーティリティで，Mac OS のパーティションサイズを小さくして, Linux用の空きスペースを作る。40GBくらい確保した。
2. Vine Linux の インストール用CD-ROMを入れてOPTキーを押しながら再起動。CD-ROMブートを選択したら GUI インストーラが立ち上がったのでそのままインスール。
- **重要: Linux パーティションは ext3 を選択する。** ext4にしたらブートしなかった。	
- grubがインストールされるが，これは起動には使わない。
3. 再起動して Mac OS を起動し，[rEFInd](http://www.rodsbooks.com/refind/getting.html)をインストール。ダウンロード・展開して以下を実行する。
```
$ ./refined-installer
```
`/Volumes/ESP/EFI`にインストールされた。
3. どこかの Linux マシンで [grub-2.0](ftp://ftp.gnu.org/gnu/grub/)をbuild。
まず，以下を実行。
```
$ ./configure --with-platform=efi --target=x86_64 --program-prefix=""
```
po関連のエラーで止まったけど気にせずに，さらに`grub-core`のbuildとインストール。
```
$ cd grub-core
$ ../grub-mkimage -O x86_64-efi -d . -o grub.efi -p "" part_gpt part_msdos ntfs ntfscomp hfsplus fat ext2 normal chain boot configfile linux multiboot
```
この`grub`をMacに持って行ってインストール。
```
$ sudo mkdir -p /Volumes/ESP/EFI/grub/x86_64-efi
$ sudo cp grub.efi /Volumes/ESP/EFI/grub/
$ sudo cp *.mod, *.lst /Volumes/ESP/EFI/grub/x86_64-efi
```
4. 再起動してペンギンマークを選択すれば Linux が起動するはず。


