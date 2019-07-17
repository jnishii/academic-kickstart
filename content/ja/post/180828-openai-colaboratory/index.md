---
title: "ColaboratoryでOpenAI gym"
date: 2018-08-28
categories: ["comp"]
tags: ["python","openAI Gym","colaboratory"]
---

[openAI Gym](https://gym.openai.com/)を[google colaboratory](https://colab.research.google.com/)で遊ぼうとすると，[Algorithm](https://gym.openai.com/envs/#algorithmic)以外の環境ではアニメーション表示がどうするかが問題になるが，一応なんとかなることがわかった。

<!--more-->

### 準備
以下をセルに入力して実行する。

```
!apt-get -qq -y install libcusparse8.0 libnvrtc8.0 libnvtoolsext1 > /dev/null
!ln -snf /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so.8.0 /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so

!apt-get -qq -y install xvfb freeglut3-dev ffmpeg> /dev/null
!pip install pyglet
!pip install pyopengl
!pip install pyvirtualdisplay
!pip install gym[classic_control]
```
次にvirtual displayの設定。

```
from pyvirtualdisplay import Display
display = Display(visible=0, size=(1024, 768))
display.start()
import os
os.environ["DISPLAY"] = ":" + str(display.display) + "." + str(display.screen)
```
これでアニメーション表示が可能になる。

### 実行

以下は[CartPole](https://gym.openai.com/envs/CartPole-v1/)(倒立振子課題)を動かす例。

```
import gym
from IPython import display
import matplotlib.pyplot as plt
%matplotlib inline

env = gym.make('CartPole-v0')

for n_episode in range (3):
    observation = env.reset()
    for t in range(100):
        plt.imshow(env.render(mode='rgb_array'))
        display.clear_output(wait=True)
        display.display(plt.gcf())

        action = env.action_space.sample() # アクションのランダム選択
        observation, reward, done, info = env.step(action)
        if done:
            print("Episode {} finished after {} timesteps".format(n_episode+1,t+1))
            break
```

これでアニメーションが表示される。表示はとても遅いが，ちょっとした確認には便利。

また，OpenAI Gymでは，上記の`env = gym.make('CartPole-v0')`を以下のように[wrappers.Monitor()](https://github.com/openai/gym/blob/master/gym/wrappers/monitor.py)を追加するとmp4で適宜保存してくれるので，学習の途中経過をあとで動画で確認できる。
保存タイミングは，env.step()を呼び出した回数が1000回以下のときには$0$, $1^3$, $2^3$, $3^3$,...と$n^3$になるごとに，それ以降は1000回ごとに保存される)

```
from gym import wrappers

env = gym.make('CartPole-v0')
env = wrappers.Monitor(env, './', force=True)
```
アニメーションは以下を実行するとダウンロードできる。

```
from google.colab import files
import glob

for file in glob.glob("openaigym.video.*.mp4"):
  files.download(file)
```

### 結果

以下は，CartPoleの課題に対してQ学習を行った結果。

これは学習前:

![](cartpole0.mp4?resize=300,200)

216エピソード学習後:

![](cartpole216.mp4?resize=300,200)

729エピソード学習後:

![](cartpole729.mp4?resize=300,200)

Q学習のソースは種々の事情により非公開ですが，例えば[Basic Reinforcement Learning Tutorial 4: Q-learning in OpenAI gym](https://github.com/vmayoral/basic_reinforcement_learning/tree/master/tutorial4)を参照すればすぐに作れます。
