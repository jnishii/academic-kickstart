---
title: "Processingでウィンドウが開かない"
date: 2017-11-22
categories: ["comp"]
tags: ["Processing"]
---

卒論プレ配属のみなさんが，Arduinoと筋電センサを使った作品を作成中。
入力データを可視化するのに[Processing](https://processing.org/)をWindows 10で使おうとした
学生さんから，描画ウィンドウが開かないとの報告。
学内LANのproxy設定をする必要があるらしい。

<!--more-->

解決策は[ここ](https://forum.processing.org/two/discussion/12578/sketches-not-running)から。

以下の設定を`%appdata%\Processing\preferences.txt`か，`Processing-3.0.1\java\lib\net.properties`に書けば良いらしい。
```
proxy.http.host=<my.proxy.ip>
proxy.http.port=<my.proxy.port>
proxy.https.host=<my.proxy.ip>
proxy.https.port=<my.proxy.port>
proxy.socks.host=<my.proxy.ip>
proxy.socks.port=<my.proxy.port>
```

ちなみに，`%appdata%`の場所の探し方は，例えば[ここ](http://www.jaskun.com/windows10/win10-appdata/)に書いてありました。