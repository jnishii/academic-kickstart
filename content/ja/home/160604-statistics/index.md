---
title: "統計検定手法の改革...その2: p値使用禁止!?"
date: 2016-06-04
categories: ["research"]
tags: ["statistics"]
---

テストの平均点がある二群で異なるかを探るには，t検定等の統計計算によってp値を求めて，優位水準より大きいか否かに基づいて有意差の有無を議論するのが一昔前まで主流だった。ところが最近は
この方法はもうやめよう...という方向に世の中は変わりつつあって，学会の投稿規定もそのように変わりつつある。

<!--more-->

- APA(アメリカ心理学会)は[論文作成マニュアル](http://www.igaku-shoin.co.jp/bookDetail.do?book=81317)(p.28)に「帰無仮説有意検定は１つの出発点に過ぎず，結果の意味を最も完全な形で伝えるには，効果量，信頼区間，それらに伴うさらなる記述などの付加的な報告要素が必要....」と明記。
<!--
[心理学系の論文誌に掲載された研究の半数以上が再現不可能という指摘](http://science.sciencemag.org/content/349/6251/aac4716.full?ijkey=1xgFoCnpLswpk&keytype=ref&siteid=sci)もある-->
- **p値使用禁止**とまで明言する論文誌([BASP, Basic and Applied Social Psychology](http://www.nature.com/news/psychology-journal-bans-p-values-1.17001))もある
- Natureにのった，p値は思ってるほど信用出来ないよという記事: [R. Muzzo, "Statiscal Errors", Nature, 506(13), 2014](http://www.nature.com/polopoly_fs/1.14700!/menu/main/topColumns/topLeftColumn/pdf/506150a.pdf)。

p値の利用は慎重に行わないといけない理由を解説した論文はいろいろある。例えば，Cumming (2014)は，2つの母集団からランダムに20個くらいのサンプルをとって検定を行うと，p値は試行ごとに大きく変わりうる。Cumming はこれを"p値のダンス"と読んで紹介している。
また，p値による議論だと有意差の有無が議論の焦点になってしまい，結局のところ比較している群間にどの程度の差があるのか，その値の信頼性はどうなのかといった議論がなくなってしまう。
例えば，二群の平均値にはほんのわずかの差しかなくても，サンプルサイズが大きいと有意差ありと判定され，そして優位差があることばかり過度に強調されることも多い。この点については，下記参考資料の[効果量，Cohen's d，検出力，検出限界](http://oku.edu.mie-u.ac.jp/~okumura/stat/effectsize.html)にわかりやすい例が紹介されている。


参考資料

- [G. Cumming, "The New Statidtics: Why and How", Psychological Science, 25(1), 7-29, (2014)](http://pss.sagepub.com/content/early/2013/11/07/0956797613504966)
    - p値は使った議論の問題点は何か，効果量や信頼区間によりどのような新たな議論が可能かを詳しく解説。
- [効果量，Cohen's d，検出力，検出限界](http://oku.edu.mie-u.ac.jp/~okumura/stat/effectsize.html)
- [効果量メモ（効果サイズ，エフェクトサイズ，effect size）](http://researchmap.jp/jo0f4s8qj-32069/?lang=japanese)
	- 参考文献等紹介が詳しくて役に立つ
