---
title: "OpenAI gym用Dockerイメージ"
date: 2018-09-01
summary: 演習用のPython環境としてDockerイメージを作った
categories: ["comp"]
tags: ["python","openAI Gym"]
---

ここのところ[openAI Gym](https://gym.openai.com/)関連の記事([8/28その1](180828-openai-colaboratory), [8/28その2](180828b-kerasrl-colaboratory), [8/29](180829-kerasrl-atari-colaboratory/))を作ったのは，
openAI Gymを遊ぶ演習をできないかなと思ったのがきっかけ。
そのためには実行環境が必要だが，大学の演習用パソコンにインストールをお願いするのはいろんな問題で結構大変。
となると，学生の皆さんが持っているパソコンにインスールする方法が候補になるが，OSは大抵 Windows になる。残念ながら，openAI Gym環境をWindowsにインストールしようとするとトラブルが多いらしく，多人数の演習での対応は難しそう。

Virtual Box上にUbuntuを走らすことも検討したが，重い上に，変なバグでキーボード操作等に不具合が生じることがあったので却下。
いろいろ考えた挙げ句，google colaboratory を使うか，Dockerでjupyter環境を用意するかが良いかなと思い，この2案を検討中。
春くらいに調べたときには，colaboratoryやjupyterでopenAI Gymのアニメーションを表示するのは無理という情報がほとんどだったが，幸いここ数ヶ月で成功事例が出てきていて，先の記事の通り，そこそこにはうまくいく。

Dockerを使う場合はローカルに起動できるのでアニメーション描画速度はかなりマシになる。
ただし，学生の皆さんのパソコンで使うのを前提でDockerイメージは作ったのでGPU計算は非対応。
google colaboratoryだと，接続のたびにアニメーション表示の設定に一手間かかり，おまけにネットワーク超えなのでアニメーション表示に難あるが，なによりもGPU計算ができる。
入門用にはDockerを使って，DeepLearning+Q学習とかを試したくなったらcolaboratoryというのがいいかな。

openAI Gymで遊ぶために作ったDockerイメージをMacにインストール・起動する手順は[ここ(日本語)](https://github.com/jnishii/docker-gym-nongpu/blob/master/Docker-howto.jp.md)に書いてある。
Dockerのインストール方法以外はLinuxでも同じ手順で使えるはず。
Dockerイメージの説明や簡単なサンプルプログラムは[ここ(英語)](https://github.com/jnishii/docker-gym-nongpu/)。
**(注: リポジトリ名を9/6/2018に変更しました)**