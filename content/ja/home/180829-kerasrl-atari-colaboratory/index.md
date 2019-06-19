---
title: "ColaboratoryでKeras-rl+OpenAI Gym (atari)"
date: 2018-08-29
categories: ["comp"]
image:
  preview_only: true
tags: ["python","openAI Gym","colaboratory"]
---


[google colaboratory](https://colab.research.google.com/)+[openAI Gym](https://gym.openai.com/)+[Keras-RL](https://keras-rl.readthedocs.io/en/latest/)の続編。今度はGymの[Atari](https://gym.openai.com/envs/#atari)を試してみた。

<!--more-->


### 準備
```
!apt-get -qq -y install libcusparse8.0 libnvrtc8.0 libnvtoolsext1 cmake> /dev/null
!ln -snf /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so.8.0 /usr/lib/x86_64-linux-gnu/libnvrtc-builtins.so

!apt-get -qq -y install xvfb freeglut3-dev ffmpeg> /dev/null
!pip install pyglet
!pip install pyopengl
!pip install gym[atari]
!pip install keras-rl
!pip install easydict
```
[前回](../180828b-kerasrl-colaboratory)のと比べると，`gym[atari]`のインストールのために1行目にcmakeを追加。
また，使ったサンプルプログラムのエラー対策のため`easydict`も追加。

### 実行

今回は，[Keras-rl](https://github.com/keras-rl/keras-rl)にある[サンプルプログラム(dqn_atari.py)](https://github.com/keras-rl/keras-rl/blob/master/examples/dqn_atari.py)を利用。
ただし，今回も[Gymのwrappers](https://github.com/openai/gym/tree/master/gym/wrappers)で動画保存をするようにした他，引数処理でエラーが出たのでその対処をしてある。
以下が修正版。もとのサンプルプログラムにあった長いコメント文は消している。

```
from __future__ import division
import argparse

from PIL import Image
import numpy as np
import easydict # added

import gym
from gym import wrappers # 追加

from keras.models import Sequential
from keras.layers import Dense, Activation, Flatten, Convolution2D, Permute
from keras.optimizers import Adam
import keras.backend as K

from rl.agents.dqn import DQNAgent
from rl.policy import LinearAnnealedPolicy, BoltzmannQPolicy, EpsGreedyQPolicy
from rl.memory import SequentialMemory
from rl.core import Processor
from rl.callbacks import FileLogger, ModelIntervalCheckpoint


INPUT_SHAPE = (84, 84)
WINDOW_LENGTH = 4


class AtariProcessor(Processor):
    def process_observation(self, observation):
        assert observation.ndim == 3  # (height, width, channel)
        img = Image.fromarray(observation)
        img = img.resize(INPUT_SHAPE).convert('L')  # resize and convert to grayscale
        processed_observation = np.array(img)
        assert processed_observation.shape == INPUT_SHAPE
        return processed_observation.astype('uint8')  # saves storage in experience memory

    def process_state_batch(self, batch):
        # We could perform this processing step in `process_observation`. In this case, however,
        # we would need to store a `float32` array instead, which is 4x more memory intensive than
        # an `uint8` array. This matters if we store 1M observations.
        processed_batch = batch.astype('float32') / 255.
        return processed_batch

    def process_reward(self, reward):
        return np.clip(reward, -1., 1.)

## argparse failes. Use easydict instead
## see https://qiita.com/LittleWat/items/6e56857e1f97c842b261
#parser = argparse.ArgumentParser()
#parser.add_argument('--mode', choices=['train', 'test'], default='train')
#parser.add_argument('--env-name', type=str, default='BreakoutDeterministic-v4')
#parser.add_argument('--weights', type=str, default=None)
#args = parser.parse_args()

args = easydict.EasyDict({ # use this instead of argparse
        "mode": "train",
        "env_name": "BreakoutDeterministic-v4",
        "weight": None,
})

# Get the environment and extract the number of actions.
env = gym.make(args.env_name)
env = wrappers.Monitor(env, './', force=True) # save animations

np.random.seed(123)
env.seed(123)
nb_actions = env.action_space.n

# Next, we build our model. We use the same model that was described by Mnih et al. (2015).
input_shape = (WINDOW_LENGTH,) + INPUT_SHAPE
model = Sequential()
if K.image_dim_ordering() == 'tf':
    # (width, height, channels)
    model.add(Permute((2, 3, 1), input_shape=input_shape))
elif K.image_dim_ordering() == 'th':
    # (channels, width, height)
    model.add(Permute((1, 2, 3), input_shape=input_shape))
else:
    raise RuntimeError('Unknown image_dim_ordering.')
model.add(Convolution2D(32, (8, 8), strides=(4, 4)))
model.add(Activation('relu'))
model.add(Convolution2D(64, (4, 4), strides=(2, 2)))
model.add(Activation('relu'))
model.add(Convolution2D(64, (3, 3), strides=(1, 1)))
model.add(Activation('relu'))
model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dense(nb_actions))
model.add(Activation('linear'))
print(model.summary())

memory = SequentialMemory(limit=1000000, window_length=WINDOW_LENGTH)
processor = AtariProcessor()

policy = LinearAnnealedPolicy(EpsGreedyQPolicy(), attr='eps', value_max=1., value_min=.1, value_test=.05,
                              nb_steps=1000000)

dqn = DQNAgent(model=model, nb_actions=nb_actions, policy=policy, memory=memory,
               processor=processor, nb_steps_warmup=50000, gamma=.99, target_model_update=10000,
               train_interval=4, delta_clip=1.)
dqn.compile(Adam(lr=.00025), metrics=['mae'])

if args.mode == 'train':
    # Okay, now it's time to learn something! We capture the interrupt exception so that training
    # can be prematurely aborted. Notice that you can the built-in Keras callbacks!
    weights_filename = 'dqn_{}_weights.h5f'.format(args.env_name)
    checkpoint_weights_filename = 'dqn_' + args.env_name + '_weights_{step}.h5f'
    log_filename = 'dqn_{}_log.json'.format(args.env_name)
    callbacks = [ModelIntervalCheckpoint(checkpoint_weights_filename, interval=250000)]
    callbacks += [FileLogger(log_filename, interval=100)]
    dqn.fit(env, callbacks=callbacks, nb_steps=1750000, log_interval=10000)

    # After training is done, we save the final weights one more time.
    dqn.save_weights(weights_filename, overwrite=True)

    # Finally, evaluate our algorithm for 10 episodes.
    dqn.test(env, nb_episodes=10, visualize=False)
elif args.mode == 'test':
    weights_filename = 'dqn_{}_weights.h5f'.format(args.env_name)
    if args.weights:
        weights_filename = args.weights
    dqn.load_weights(weights_filename)
    dqn.test(env, nb_episodes=10, visualize=True)
```

### 結果

このプログラムでは1,750,000 stepsの学習をするように設定してあったが，380,000 steps 終了した後に`Buffered data was truncated after reaching the output size limit.`と出てとまってしまった。batch sizeをいじったり，途中経過を保存しつつ，小分けに計算するなどすればいいのだろうが対策はしていない。
一応学習は進んでいるみたい。

学習前

<!--![](atari-kerasrl-0.mp4?resize=300,200)-->
{{<video src="atari-kerasrl-0.mp4" width="300" >}}


学習中(どの時点のものか未解読)

<!--![](atari-kerasrl-2000.mp4?resize=300,200)-->
{{<video src="atari-kerasrl-2000.mp4" width="300" >}}


### リンク
- [作成したノートブック@Colaboratory](https://colab.research.google.com/drive/12CI05SWit03mjdGc3DHJr5k-0gVDfibK)

