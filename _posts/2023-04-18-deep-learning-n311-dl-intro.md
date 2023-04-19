---
title:  "AIB 딥러닝 311 인공신경망 인트로"
excerpt: "딥러닝 시작. 딥러닝의 기본 구조를 알아보자."
header:
  teaser: /assets/images/s3dl11-deep-learning.png
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, neural_network]
date: 2023-04-18 19:00
last_modified_at: 2023-04-18T19:00:00-09:00
---

<p align="center">
  <img src="/assets/images/s3dl11-deep-learning.png" />
</p>

적은 개수의 알고리즘으로 소수의 단계를 거쳐 회귀/분류 문제를 풀어낸 것이 머신러닝이었다면 아주 많은 단계를 거쳐 답을 도출해내는 것이 딥러닝이다. 대충 말해서 그렇고, 세부 구조를 살펴보면 다를 것이다.

입력값을 받아 결과를 내는 것을 생각해볼 수 있다. 가장 작은 단위의 이것을 퍼셉트론이라 한다. 이것이 하나만 있다면 단순한 문제는 답을 낼 수 있지만, 복잡한 문제는 답을 제시할 수 없다. 예를 들어 이진분류문제에서 직선의 threshold를 기준으로 True/False를 정하는 것은 가능한데, 어떤 원점에서부터 Quadrants 1,3과 2,4로 구분된다면 하나의 직선으로 구분해낼 수 없다. 직선이 두 개는 있어야 한다. 그래서 은닉층 (hidden layer)을 더해 하나의 퍼셉트론으로 풀 수 없던 문제를 다중퍼셉트론으로 풀 수 있다.

다중퍼셉트론은 입력, 은닉, 그리고 출력층으로 구분된다. 각각의 층은 노드가 있는데, 특성과 타겟에 따라 개수가 다르다. 입력층은 특성의 개수만큼의 노드가 있고, 출력층은 다중분류면 클래스 개수만큼, 이진분류면 1개, 그리고 회귀문제면 보고자 하는 결과 개수 (타겟이라 부르지 않고 feature라고 부르는데 가만 보니 왜 그런지 모르겠다. 심지어 오늘 회귀 문제 하나도 안 풀어봄). 은닉층은 아무 개수나 아무 연산이나 가능하며, 일반적으로 너무 복잡하게 구성되어 있어서 사용자가 알 수 없다 한다. 물론 은닉층을 잘 구성하고 초기값을 잘 주어야 좋은 결과가 나올 것이다.

은닉층에서는 가중치와 편향이 계산된다. 뭐가 가중치이고 편향인지는 모르겠다. 가중치는 더 중요하게 여기는 값 정도로 이해되는데, 다음 노트에서 더 자세한 설명이 나온다고 한다. 이런 연산 중에서 함수도 사용된다.

어떤 함수를 적용하느냐 하면 대표적으로 sigmoid, relu, 그리고 softmax가 있다. sigmoid는 0과 1을 구분해주는 함수로, 연속성이 있지만 반복 사용시 기울기가 0으로 간다. relu는 rectified linear unit으로, 0 이전에는 0, 이후에는 y=x의 형태로, sigmoid의 기울기 0으로의 수렴의 단점을 보완해준다고 한다. softmax는 sigmoid 함수에서 결과 개수만큼 지수함수가 늘어나서, 출력층에서 사용시 다중분류의 결과를 얻을 수 있다. 강의 노트에는 없었지만 강사님이 `tanh`도 언급하셨는데, sigmoid와 형태는 비슷한데 -1까지 내려간다. 뭐가 좋은지 설명이 안 되지만, 기울기는 마찬가지로 0으로 갈 것이다. 기울기가 소실되는게 왜 문제인지는 나중에 알려준다고 한다. 이런 함수들을 활성화 함수, activation functions라 한다. 전부 선형함수가 아닌 비선형함수인데, 선형함수는 몇 번 operator를 적용해도 자기 자신과 같은 형태가 나오기 때문에 쓸모 없어서 비선형을 사용한다. 결과를 받을 때 이진분류는 로지스틱 회귀에서 봤듯이 sigmoid이고, 다중분류는 sigmoid 여러 개의 개념인 softmax, 그리고 회귀는 활성화 함수를 사용하지 않는다고 하는데, 이런 구분 없이 연속적인 값을 받기 위해 그런 것 같다.

일단은 `TensorFlow`의 `keras`로 시작했다. 교육용으로 매우 간단한 모듈이라고 한다. 사용 방법도 크게 두 가지로 구분하는데, Sequential API로 하나씩 더하는 것과 Functional API로 하나의 layer마다 변수로 저장해주는 것이 있다. 전자는 입력층을 지정해주지 않고 하나씩 더하는 간단한 부분이 있고, 후자는 입력층도 지정해주며 각 층마다 살펴볼 수 있다고 한다.

```python
'''코드 일부분'''
import tensorflow as tf

def my_dl(nodes_in_layers, activation_funcs, epochs=5):
  # Sequential API
  
  # # 방법 1
  # model = tf.keras.models.Sequential([
  #     tf.keras.layers.Dense(100, activation='relu'),
  #     tf.keras.layers.Dense(1, activation='sigmoid')
  # ])

  # 방법 2
  model = tf.keras.models.Sequential()
  for idx, nodes in enumerate(nodes_in_layers): # 각 layer마다 노드 개수와
    model.add(tf.keras.layers.Dense(nodes, activation=activation_funcs[idx])) # 활성화함수 지정
  model.add(tf.keras.layers.Dense(1, activation='sigmoid')) # 마지막은 이진분류 출력

  # # Functional API
  # input = tf.keras.layers.input(shape=(69,))
  # hidden = tf.keras.layers.Dense(50, activation='relu')(input)
  # output = tf.keras.layers.Dense(1, activation='sigmoid')(hidden)
  # model = tf.keras.models.Model(inputs=input, outputs=put)

  # 모델 컴파일
  model.compile(
      optimizer='sgd', # 모름
      loss='binary_crossentropy', # 모름
      metrics=['accuracy'] # 평가지표 어느 것으로 높일지
  )

  # 학습 및 결과 반환
  model.fit(X_train, y_train, epochs=epochs)
  y_proba_test = model.predict(X_test) # 확률 반환
  y_pred_test = y_proba_test > 0.5

  for metric in [accuracy_score, precision_score, recall_score, f1_score]:
    print(metric.__name__, metric(y_test, y_pred_test))
  
  return model
```
중간에 보이지만, 머신러닝은 대부분 `.predict(X)`로 예측값을 받고, `.predict_proba(X)`로 확률을 받았는데, 딥러닝은 `predict(X)`로 확률을 받고 있다. 참 헷갈린다.

머신러닝은 pipeline 개념으로 무엇을 할지 순차적으로 정해주고 한 번 돌려서 결과를 보는 식으로 생각할 수 있는데, 딥러닝에서 `TF`의 `keras`는 순서도 정해주고 compile도 따로 해주고 단계가 하나 더 복잡해졌다. 쉬운거 하나 없다.