---
title: "統計検定手法の改革...その3: 効果量, 信頼区間"
date: 2016-06-05
mathjax: true
categories: ["research"]
tags: ["statistics"]
---

p値にかわる統計的手法として，とりあえずの候補としてよく紹介されるのは，効果量と信頼区間に基づく議論。というわけで，必要に迫られてお勉強した内容の覚書。

<!--more-->

## 効果量の定義

独立な2群の平均値の差を議論する場合には ，効果量(Effect Size, ES)として Cohen's d もしくはHedges' g (unbiased Cohen's d)を使うことが多い。
いずれも，二群のサンプル平均の差がpooled standard deviation (各群のサンプルサイズを考慮した標準偏差の平均値)に対してどの程度かを表す。
つまり，両群の標準偏差が等しい場合に d=1 ならば，両群のサンプル平均の差がちょうど標準偏差に等しいことになる。

これらの量の定義式が文献によって微妙に違うことがあってややこしいが，
Nakagawa and Cuthill (2007) のTable 1 によると以下の通り。

Cohen's d:

\\[d=\frac{\bar{x}_1-\bar{x}_2}{s}\\]

\\[s=\sqrt{\frac{(n_1-1)s_1^2+(n_2-1)s_2^2}{n_1+n_2-2}}\\]

Hedges' g (unbiased Cohen's d):

\\[g\simeq \left(1-\frac{3}{4(n_1+n_2-9)}\right)d\\]

\(s_1^2\), \(s_2^2\)は各群の不偏分散，\(n_1\), \(n_2\)はサンプルサイズ。
補正項のあるHedges'gを用いることが多くの文献で推奨されている。
また，Cumming (2014)はこれを Cohen's d　と記した上で，その定義式もしくはその出典を明記することを勧めている。

参考文献

- [G. Cumming, "The New Statidtics: Why and How", Psychological Science, 25(1), 7-29, (2014)](http://pss.sagepub.com/content/early/2013/11/07/0956797613504966)
- [S. Nakagawa and I. C. Cuthill, "Effect size, confidence interval and statistical significance: a practical guide for biologists", Biol. Rev. 82(4), 591-605 (2007)](http://www.ncbi.nlm.nih.gov/pubmed/17944619)


## 効果量の信頼区間

[Nakagawa and Cuthill (2007)](http://www.ncbi.nlm.nih.gov/pubmed/17944619)の Table 3 の(17)式によると，Cohen's dの標準誤差は以下の通り。

\\[
\sigma_d=\sqrt{\frac{n_1+n_2}{n_1n_2}+\frac{d^2}{2(n_1+n_2-2)}}
\\]

効果量の95%信頼区間(Confidence Interval, CI)は以下になる。

\\[d\in[d-1.96\sigma_d,d+1.96\sigma_d]\\]

もし，効果量が d=0.6 [0.3,0.9] ならば信頼区間がd=0(平均値の差がなし)を含まないので，有意水準5%で有意差がありといえる。
ただし，はじめの方にも触れたように, [Cumming (2014)]((http://pss.sagepub.com/content/early/2013/11/07/0956797613504966))は，信頼区間を報告しても結局有意差の有無しか議論しないのならp値を使った議論となんら変わらないので, 効果量の大きさや信頼区間の値に基づく議論をきちんとするようにと説明している。


## 効果量の計算

効果量や信頼区間の計算するツールは，googleで検索するといろいろある。
でも，以下のRの項でも説明するように，同じ値を計算しているはずでも，計算ツールによって何故か違う値が出てくることがある。
ちょっとお試しの時はともかく，研究発表の時にどのツールを使うかは慎重に検討する必要がある。
今のところ一番無難なのは，上述の定義式にしたがって自分で計算することのように思うが，
一応代表的なものをいくつか紹介。

### ESCI

[ESCI(Exploratory Software for Confidence Intervals)](http://www.latrobe.edu.au/psychology/research/research-areas/cognitive-and-developmental-psychology/esci/understanding-the-new-statistics)
は Cummingが公開しているエクセルシート
	- Cummingの著書にあるいろんな計算をできるようになっていて，著書を持っていなくても面白い
	- ただ，自分のデータの解析には使いにくい(使い方をきちんとわかっていないだけかもしれないが)

### RでCohen's dの計算

#### 方法1: パッケージcompute.esの関数mesを使う

```R
install.packages("compute.es") # 初めて使うときのみ
library(compute.es)
mes(mean1, mean2, sd1, sd2, n1, n2)
```

関数mesで出力される値

- Cohen's d と Hedges's g の値は上記の定義式通り
- Cohen's dの分散としては，(多分)上記の「効果量の信頼区間」に記載した標準誤差の二乗\(\sigma_d^2\)が出力されている。
- Hedges's gの分散も上式による値に近いが，微妙に違うような...
- Cohen's d, Hedges's g ともにCIは計算が合わない(少なくともCIは　\(1.96\sigma_d\) より大きな値になっている。


#### 方法2: バッケージeffsizeの関数cohen.dを使う

[effsize](https://cran.r-project.org/web/packages/effsize/index.html)には，いろいろな効果量を計算できるパッケージ。

```
install.packages("effsize") # 初めて使うときのみ
library(effsize)
cohen.d(data1, data2) # Cohen's d
cohen.d(data1, data2, hedges.correction=TRUE) # Hedges's g
```
- Cohen's d と Hedges's g の値は上記の定義式通り。
- CIの計算方法は不明。上記定義式とcompute.esの関数mesのいずれよりも大きくなる。


#### パッケージcompute.esとeffsizeの比較を行った結果

N(0,1)とN(1,1)の母集団から10個づつランダムにサンプルをとって，Cohen's dを以下の三通りで計算してみた。

1. 上記定義式
2. パッケージeffsizeの関数cohen.d
3. パッケージcompute.esの関数mes


```R
[[From definition]]----------------
[1] "Cohen's d   : 0.416873 [-0.470181, 1.303927]"
[1] "  squared SE: 0.204827"
[1] "unbiased Cohen's d: 0.399259 [-0.486930, 1.285447]"
[1] "  squared SE      : 0.204428"

[[Using effsize]]------------------

Cohen's d

d estimate: 0.4168729 (small)
95 percent confidence interval:
       inf        sup 
-0.5853922  1.4191380 

Hedges's g

g estimate: 0.3992585 (small)
95 percent confidence interval:
       inf        sup 
-0.6020291  1.4005462 

[[Using compute.es]]---------------
Mean Differences ES: 
 
 d [ 95 %CI] = 0.42 [ -0.53 , 1.37 ] 
  var(d) = 0.2 
  p-value(d) = 0.37 
  U3(d) = 66.16 % 
  CLES(d) = 61.59 % 
  Cliff's Delta = 0.23 
 
 g [ 95 %CI] = 0.4 [ -0.51 , 1.31 ] 
  var(g) = 0.19 
  p-value(g) = 0.37 
  U3(g) = 65.51 % 
  CLES(g) = 61.12 % 
```
上記の結果を出すのに使ったプログラムは以下のとおり。
```R
n1=n2=10
dat1<-rnorm(n1,mean=1)
dat2<-rnorm(n2)
m1<-mean(dat1)
m2<-mean(dat2)
sd1<-sd(dat1)
sd2<-sd(dat2)

pooled_s<-sqrt(((n1-1)*sd1^2+(n2-1)*sd2^2)/(n1+n2-2))
cohend<-(m1-m2)/pooled_s
d_SE<-sqrt((n1+n2)/(n1*n2)+cohend^2/(2*(n1+n2-2)))
d_CI=1.96*d_SE

unbiased_cohend<-cohend*(1-3/(4*(n1+n2)-9))
ub_SE<-sqrt((n1+n2)/(n1*n2)+unbiased_cohend^2/(2*(n1+n2-2)))
ub_CI=1.96*ub_SE

cat("[[From definition]]----------------\n")
sprintf("Cohen's d   : %f [%f, %f]", cohend, cohend-d_CI, cohend+d_CI)
sprintf("  squared SE: %f", d_SE^2)
sprintf("unbiased Cohen's d: %f [%f, %f]", 
	unbiased_cohend, unbiased_cohend-ub_CI, unbiased_cohend+ub_CI) 
sprintf("  squared SE      : %f", ub_SE^2)

cat("\n[[Using effsize]]------------------\n")
library(effsize)
cohen.d(dat1,dat2)
cohen.d(dat1,dat2,hedges.correction=TRUE)

cat("\n[[Using compute.es]]---------------\n")
library(compute.es)
mes(m1,m2,sd1,sd2,n1,n2)
```


## おまけ

「新しい検定方法」についてのCummingの講義がYoutubeに公開されています。

- [The New Statistics: Confidence Intervals, NHST, and p Values (Workshop Part 1)](https://www.youtube.com/watch?v=iJ4kqk3V8jQ)
- [The New Statistics: Research Integrity & the New Statistics (Workshop Part 2)](https://www.youtube.com/watch?v=wb0rnZBlcRg)

ほかにもいろいろ
- ["Cumming" "new statistics" でYoutubeを検索](https://www.youtube.com/results?search_query=Cumming+new+statistics)
