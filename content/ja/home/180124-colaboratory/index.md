---
title: "ColaboratoryでGPU計算"
date: 2018-01-24
categories: ["comp"]
tags: ["python","colaboratory","jupyter"]
---

pythonのコードを少し試す時には[jupyter notebook](http://jupyter.org/)が便利なんだけど，お手軽にクラウド上で使えるものが無いかと探したら，google の[Colaboratory](https://colab.research.google.com/notebook)が無料で使えて，共同編集もできるので便利。
GPU計算にも(多分)2日前位に対応したらしく，tensorflowとかKerasとかも使えるので，ディープラーニングでも遊べる。

<!--more-->

### CPUの情報
```
!cat /proc/cpuinfo
```
```
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 63
model name	: Intel(R) Xeon(R) CPU @ 2.30GHz
stepping	: 0
microcode	: 0x1
cpu MHz		: 2300.000
cache size	: 46080 KB
physical id	: 0
siblings	: 2
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms xsaveopt
bugs		:
bogomips	: 4600.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 63
model name	: Intel(R) Xeon(R) CPU @ 2.30GHz
stepping	: 0
microcode	: 0x1
cpu MHz		: 2300.000
cache size	: 46080 KB
physical id	: 0
siblings	: 2
core id		: 0
cpu cores	: 1
apicid		: 1
initial apicid	: 1
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms xsaveopt
bugs		:
bogomips	: 4600.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
```
このCPUパワーを独占できるわけではないはずだが，いまのところ手元のMacBookと比べるとかなり速い(実行タイミングにより数割から数倍早い)

### GPUの情報
```
!cat /proc/driver/nvidia/gpus/0000:00:04.0/information
```
```
Model: 		 Tesla K80
IRQ:   		 33
GPU UUID: 	 GPU-be0a1aaf-c92d-ce5e-6d53-1f064e6842f6
Video BIOS: 	 80.21.25.00.01
Bus Type: 	 PCI
DMA Size: 	 40 bits
DMA Mask: 	 0xffffffffff
Bus Location: 	 0000:00:04.0
Device Minor: 	 0
```
Kerasで作ったディープラーニングのプログラムを走らせると，GPUを使わない場合に比べて10から20倍早い。
Kerasのサンプルプログラムは[こちら](http://bcl.sci.yamaguchi-u.ac.jp/~jun/notebook/Keras)にあります。
