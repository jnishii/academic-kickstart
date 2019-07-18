---
title: "ColaboratoryでKeras-rl+OpenAI Gym (classical_control)"
date: 2018-08-28
image:
  preview_only: true
categories: ["comp"]
tags: ["python","openAI Gym","colaboratory"]
---

{{< video src="cartpole-kerasrl-216.mp4" width="300">}}

[google colaboratory](https://colab.research.google.com/)上で，[openAI Gym](https://gym.openai.com/)の[Classic Control](https://gym.openai.com/envs/#classic_control)を使って遊べることがわかったので，さらに[Keras-RL](https://keras-rl.readthedocs.io/en/latest/)を使ってDQL(Deep-Q Learning)を試してみた。colaboratoryは[Keras](https://keras.io/ja/)をサポートしているので，あっけなくデモが動いてめでたし。

<!--more-->
### 準備
必要なものをインストール。[前の記事](../180828-openai-colaboratory)とほぼ同様だが，keras-rlも追加。また，今回はcolaboratory上にアニメーションは表示しないことにするので，pyvirtualdisplayはインストールしない。

```
!apt-get -qq -y install libcusparse8.0 libnvrtc8.0 libnvtoolsext1 > /dev/null
!ln -snf /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so.8.0 /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so

!apt-get -qq -y install xvfb freeglut3-dev ffmpeg> /dev/null
!pip install pyglet
!pip install pyopengl
!pip install gym[classic_control]
!pip install keras-rl
```

### 実行

[Keras-rl](https://github.com/keras-rl/keras-rl)にある[サンプルプログラム(dqn_cartpole.py)](https://github.com/keras-rl/keras-rl/blob/master/examples/dqn_cartpole.py)を利用。
ただし，[前の記事](../180828-openai-colaboratory)に書いたように[Gymのwrappers](https://github.com/openai/gym/tree/master/gym/wrappers)を使う方法で，動画保存をしている。ソースは以下の通り。元のサンプルプログラムにあった長いコメントは消してある。
実行前には**GPUアクセラレーションを有効に**するのを忘れずに。Colaboratoryのメニュー「ランタイム/ランタイムのタイプを変更」で設定する。

```
import numpy as np
import gym

from keras.models import Sequential
from keras.layers import Dense, Activation, Flatten
from keras.optimizers import Adam

from rl.agents.dqn import DQNAgent
from rl.policy import BoltzmannQPolicy
from rl.memory import SequentialMemory

from gym import wrappers # <=追加

ENV_NAME = 'CartPole-v0'

# Get the environment and extract the number of actions.
env = gym.make(ENV_NAME)
env = wrappers.Monitor(env, './', force=True)  # <=追加

np.random.seed(123)
env.seed(123)
nb_actions = env.action_space.n

# Next, we build a very simple model.
model = Sequential()
model.add(Flatten(input_shape=(1,) + env.observation_space.shape))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(nb_actions))
model.add(Activation('linear'))
print(model.summary())

memory = SequentialMemory(limit=50000, window_length=1)
policy = BoltzmannQPolicy()
dqn = DQNAgent(model=model, nb_actions=nb_actions, memory=memory, nb_steps_warmup=10,
               target_model_update=1e-2, policy=policy)
dqn.compile(Adam(lr=1e-3), metrics=['mae'])
dqn.fit(env, nb_steps=50000, visualize=True, verbose=2)
dqn.save_weights('dqn_{}_weights.h5f'.format(ENV_NAME), overwrite=True)

# Finally, evaluate our algorithm for 5 episodes.
# dqn.test(env, nb_episodes=5, visualize=True) # これはエラーが出るのでコメントに
```

最後の行は，エピソードのステップ数を調整すればエラーはなくなるはずだが，未対応。

### 結果

学習前

<!--![](cartpole-kerasrl-0.mp4?resize=300,200)-->

{{< video src="cartpole-kerasrl-0.mp4" width="300">}}

64エピソード後

<!--![](cartpole-kerasrl-64.mp4?resize=300,200)-->

{{< video src="cartpole-kerasrl-64.mp4" width="300">}}

216エピソード後

<!--![](cartpole-kerasrl-216.mp4?resize=300,200)-->
{{< video src="cartpole-kerasrl-216.mp4" width="300">}}

[MuJoCo](https://gym.openai.com/envs/#mujoco)の楽しげな物理シミュレシーションで遊ぼうと，Keras-rlの[デモ(ddpg_mujoco.py)](https://github.com/keras-rl/keras-rl/blob/master/examples/ddpg_mujoco.py)を動かそうとしたが，そもそもMuJoCoの導入がうまくいかない。
MuJoCoはライセンスの問題とかもあるので，さっさと[Roboschool](https://blog.openai.com/roboschool/)で試してみるか。

## リンク

- [作成したノートブック@Colaboratory](https://colab.research.google.com/drive/1yeTcvE3qE2VQvm0TpQTLlalIOlv1h3On)