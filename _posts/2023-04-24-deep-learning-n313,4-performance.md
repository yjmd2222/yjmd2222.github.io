---
title:  "AIB 딥러닝 313,4 인공신경망 성능 향상"
excerpt: "인공신경망의 성능을 더욱 높이기 위한 방법들을 알아보자"
header:
  teaser: /assets/images/s3dl13-deep-learning.png
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, neural_network, 'gradient_descent', 'hyperparameter_tuning']
date: 2023-04-24 19:00
last_modified_at: 2023-04-24T19:00:00-09:00
---

N313에서는 모델 성능을 높이기 위한 방법들로 학습률 계획, 가중치 초기화, 가중치 감소, 드롭아웃, 그리고 조기종료가 있었다. N314에서는 이를 포함한 여러 가지를 조합해서 최적의 성능을 내는 하이퍼파라미터 튜닝에 대해서 배웠다.

학습률 계획은 학습률에 변동을 주어 오차함수의 local minimum을 벗어나 global minimum을 찾아갈 것을 기대하는 일종의 방법이다. 복잡한 계획법이 가능하겠지만, 강의노트에서는 학습률을 큰 값에서 작은 값으로 감소시키는 것을 다룬다. 적용해보기 전에는 성능이 향상될지 아닐지 전혀 모른다.

가중치는 여러 번 활성화 함수를 거치면 gradient가 소실될 수 있다. 가중치를 초기화해주면 소실되는 것을 방지할 수 있다. 대표적으로 Xavier는 sigmoid 신경망에 적용, He는 relu 신경망에 적용한다. `kernel_initializer`로 적용 가능하고, 기본적으로 Xavier(Glorot) uniform이 적용되어 있다.

가중치 감소는 머신러닝의 규제화와 같은 개념이다. L1 또는 L2 규제화로 가중치가 너무 커지지 않도록 페널티를 부과하게 된다. 큰 범위에서 가중치 감소라고 표현하지만, `kernel_regularizer`는 가중치, `bias_regularizer`는 편향, 그리고 `activity_regularizer`는 활성화함수를 거친 결과를 감소시킨다.

드롭아웃은 레이어 노드 중 일부를 사용하지 않으면서 학습을 진행하는 것이다. iteration마다 랜덤하게 노드를 사용하지 않으므로 일반화 성능이 올라간다. `.layers.Dropout`으로 사용한다.

조기종료는 머신러닝과 마찬가지로 검증 성능이 좋아지지 않으면 / 검증 손실이 줄어들지 않으면 멈추는 기능이다. `.keras.callbacks` 모듈의 하위 함수들로 사용이 가능하고, 별도의 callback은 `.callbacks.Callback` 클래스로 만들어서 사용이 가능하다.

하이퍼파라미터 탐색은 batch_size, epochs, optimizer, learning_rate, activation, regularizer, dropout, number of nodes가 대표적이다. scikit-learn과 연결하여 사용하는 방법이 있고, 연결하지 않고 탐색하는 Keras Tuner도 있다.

```python
from tensorflow.keras.datasets import fashion_mnist
from tensorflow.keras.models import Sequential, Model
from tensorflow.keras.layers import Dense, Flatten, Dropout, Input
from tensorflow.keras import regularizers

import os
import numpy as np
import tensorflow as tf
import keras

np.random.seed(42)
tf.random.set_seed(42)

(X_train, y_train), (X_test, y_test) = fashion_mnist.load_data()

X_train = X_train / 255. # 이미지 정규화
X_test = X_test / 255. # 이미지 정규화

# 학습률 계획
first_decay_steps = 1000 # step = iteration
initial_learning_rate = 0.01 # 초기 학습률

lr_decayed_fn = (
  tf.keras.experimental.CosineDecayRestarts( # 코사인 학습률 감소 계획
      initial_learning_rate,
      first_decay_steps)
  )
     
# 가중치 감소
model = Sequential([
    Flatten(input_shape=(28, 28)),
    Dense(64,
          kernel_regularizer=regularizers.l2(0.01), # 가중치 감소: 가중치에만
          bias_regularizer=regularizers.l2(0.001), # bias에
          activity_regularizer=regularizers.l1(0.01)), # output에
    Dense(100, activation='relu', kernel_initializer='he_uniform')
    Dropout(0.5), # drop out rate
    Dense(10, activation='softmax')
])

# model = Sequential()
# model.add(Flatten(input_shape=(28,28)))
# model.add(Dense(64, kernel_regularizer=regularizers.l2(0.01), activity_regularizer=regularizers.l1(0.01)))
# model.add(Dense(100, activation='relu', kernel_initializer='he_uniform'))
# model.add(Dropout(0.5))
# model.add(Dense(10, activation='softmax'))

# my_input = Input(shape=(28, 28))
# my_flat = Flatten()(my_input)
# my_hidden_1 = Dense(64, kernel_regularizer=regularizers.l2(0.01), activity_regularizer=regularizers.l1(0.01))(my_flat)
# my_hidden_2 = Dense(100, activation='relu', kernel_initializer='he_uniform')(my_hidden_1)
# my_drop = Dropout(0.5)(my_hidden_2)
# my_output = Dense(10, activation='softmax')(my_drop)
# model = Model(inputs=my_input, outputs=my_output)

model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=lr_decayed_fn, beta_1 = 0.89) # 학습률 적용. beta_1은 Adam에 적용되는 파라미터
             , loss='sparse_categorical_crossentropy'
             , metrics=['accuracy'])

model.summary() # shape나 dim 넣어야함.

checkpoint_filepath = "FMbest.hdf5"
early_stop = keras.callbacks.EarlyStopping(monitor='val_loss', min_delta=0, patience=10, verbose=1) # min_delta: 얼마만큼 손실이 줄었는지; patience: 손실 개선 안 되는 것 몇 epoch 기다릴지

# save_weights_only: 가중치/편향만 저장. layer 저장 안 한다고 함. 나중에 찾아볼 것.
# mode : 검증 지표가 val_acc일 경우 정확도이기 때문에 높을 수록 좋기 때문에 'max'로 설정, val_loss일 경우 낮을 수록 좋기 때문에 'min'으로 설정, 'auto'의 경우 자동으로 탐지하여 진행함.
# save_freq : epoch / integer. epoch마다 저장할지 아니면 iteration마다 (number of batches) 저장할지
save_best = tf.keras.callbacks.ModelCheckpoint(
    filepath=checkpoint_filepath, monitor='val_loss', verbose=1, save_best_only=True, # 'val_loss' validation 기준으로 early stopping 진행
    save_weights_only=True, mode='auto', save_freq='epoch', options=None)

model.fit(X_train, y_train, batch_size=32, epochs=30, verbose=1, 
          validation_data=(X_test,y_test), # 검증 데이터
          callbacks=[early_stop, save_best]) # callbacks는 여기에 모조리 넣는다.

model.predict(X_test[0:1])

test_loss, test_acc = model.evaluate(X_test,  y_test, verbose=2)

model.load_weights(checkpoint_filepath)

model.predict(X_test[0:1]) # 테스트 실행

test_loss, test_acc = model.evaluate(X_test,  y_test, verbose=1) # 테스트 지표 확인
```

튜너는 종류가 여럿 있다. scikeras, Keras Tuner, 또 tf 안에 scikit_learn 이름으로 구현한 모듈. 각각마다 작동 방식도 다르고 명시해도 되는 것 / 안 해도 되는 것이 차이가 있다. `model.summary`와 마찬가지로 scikeras는 입력 shape/dim을 입력해야 한다.

```python
# scikeras
# !pip install scikeras
import numpy
import pandas as pd
from sklearn.model_selection import GridSearchCV
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from scikeras.wrappers import KerasClassifier

'''
데이터셋 준비
'''

# 튜너들이 값을 변경해줄 수 있는 변수를 인자로 하는 함수 생성해서 모델 반환하기
# 내 마음대로 변수 정의할 수 있어서 편리할 것 같다.
def create_model(nodes=8, activation_1='relu'):
    model = Sequential()
    model.add(Dense(nodes, input_dim=8, activation=activation_1))
    model.add(Dense(nodes, activation='relu'))
    model.add(Dense(1, activation='sigmoid'))
    
    model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
    return model

model = KerasClassifier(model=create_model, batch_size=8)
nodes = [16, 32, 64]
batch_size = [16, 32, 64]
activation_1 = ['relu', 'tanh', 'sigmoid']
lrs = [0.01, 0.05, 0.1, 0.3]
optimizers = ['adam', 'sgd', 'adagrad']
param_grid = {
  'model__nodes': nodes, # create_model의 인자들은 model__ 앞에 적어준다.
  'batch_size': batch_size,
  'model__activation_1': activation_1,
  'optimizer': optimizers,
  'optimizer__learning_rate': lrs,
}

grid = GridSearchCV(estimator=model, param_grid=param_grid, n_jobs=1, cv=3)
grid_result = grid.fit(X, Y)
```