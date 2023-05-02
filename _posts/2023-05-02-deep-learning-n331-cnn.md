---
title:  "AIB 딥러닝 331 합성곱 신경망"
excerpt: "정교한 이미지 분석을 위한 합성곱 신경망을 배워보자"
header:
  # teaser:
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, convolutional_neural_network, transfer_learning]
date: 2023-05-02 20:00
last_modified_at: 2023-05-02T20:00:00-09:00
---

합성곱 신경망(Convolutional Neural Network)는 이미징 분석에서 2차원의 그림의 정보를 잘 담아낸다. 그림을 조각내서 1차원으로 이어붙이는 `Flatten`은 공간의 특성을 살리지 못하는데, CNN은 가능하다.

CNN에서 층은 Convolution layer와 pooling layer가 있고, 결과를 낼 때 차원을 축소하는 방식으로 `GlogalAveragePooling2d`도 있다.

Convolution layer는 이미지를 조각냈을 때 그 중 일부(patch)를 가중치와 곱해 sliding하면서 convolution matrix를 만든다. 일부를 전문 용어로 filter 또는 kernel 할 수 있다. 이 커널은 전체 이미지의 일부이고, 그 일부를 매핑하면서 convolution matrix/Feature Map를 만들 때 옮겨가면서 conv mat의 element를 구하는 것을 sliding이라고 한다. sliding할 때 몇 칸씩 옮길지에 대한 변수를 stride라고 한다.

만약 이미지를 있는 그대로 sliding한다면 conv mat은 이미지보다 사이즈가 줄어들게 된다. 이미지에 padding을 더한다면 conv mat의 사이즈를 늘려 최대 원래 이미지와 같은 사이즈로 만들 수 있다. 이를 수식화하면 다음 결과를 얻을 수 있다:

$$ N_{\text{out}} = \bigg[\frac{N_{\text{in}} + 2p - k}{s}\bigg] + 1 $$

- N: 이미지 크기. 피처 수
- k: 필터/커널 크기
- p: 패딩 값
- s: 스트라이드 값

가로 세로 각각 계산한다.

pooling layer는 입력된 matrix를 축소시키는 층이다. 축소 방식은 여러 가지가 있겠지만, 대표적으로는 max pooling과 average pooling이 있다. 큰 덩어리로 입력 matrix를 나누었을 때 하나의 조각 안의 최대값이나 그 평균값을 뽑아서 새로운 matrix에 기록하는 것. max는 보고 싶은 특징을 잘 잡아내고, average는 smooth하게 만든다.

pooling은 가중치 연산이 없고, 축소시킬 때 채널을 축소시키지 않는다. 채널은 RGB에서 RGB를 하나로 합치지 않는다고 생각할 수 있다.

완전 연결 계층 fully connected layer은 기존 다층 퍼셉트론이라고 생각할 수 있다. 이미지 학습 끝에 dense로 회귀/분류문제에 대해 학습하는 것이다. 그러기 위해 이미지를 1차원으로 나열하는 `Flatten`을 사용한다.

여기까지 매우 간단하게 과정을 적어보았는데, 세부적으로 몇 개의 커널로 몇 개의 채널을 만드는지 등은 매우 헷갈린다. 다시 정리해보자.

- convolution layer에서 filter로 feature map을 만든다. 몇 개의 feature map이 만들어지는가? $ \rm{num \  channels} \times \rm{num \  filters} $.
- convolution layer에 채널이 세 개 있다. output에 커널 개수에 영향을 주는가? 전혀 다른 문제다. output 커널 개수는 내가 정해준다.
- 가중치는 몇 개인가? $ \rm{size \  filter \times num \  features \times num \ channels} $
- 어떻게 feature map이 output 커널로 mapping되는가? 하나의 feature map은 입력 채널마다 *동일한 가중치*로 계산된 matrix를 더해주어 만든다.
- 방금 전은 일반적인 (standard) convolution이다. depthwise convolution은 각각의 채널에 대해 feature map을 보유한다.
- $ x \times y $에 대해서가 아니라 $ z $에 해당하는 채널들에 대해 convolution을 진행할 수 있다. $ 1 \times 1 times c $로 map에 수직한 방향에서 바라보면 1 개만 고르는데, 모든 채널들에 대해 골라서 하나로 압축시켜 채널 수를 감소시킬 수 있다. 이를 pointwise convolution이라 부른다.
- depthwise separable convolution은 depthwise convolution 다음 pointwise convolution을 진행한 것이라 볼 수 있다. filter size를 동일하게 설정한다면 결과적으로는 standard convolution과 동일한 형태를 가질 수 있다. 그런데 차원을 축소했다가 키우기 때문에 연산량이 훨씬 적다.

`Flatten` 대신 `GlobalAveragePooling2d`를 사용할 수 있다. 마찬가지로 채널 당 하나의 값으로 축소시키기 때문에 연산량이 엄청 줄어든다.

```python
# 입력 데이터의 차원: (batch_size, height, width, channels)
x = tf.random.normal((32, 224, 224, 3))

# GlobalAveragePooling2D 계층 생성
y = tf.keras.layers.GlobalAveragePooling2D()(x)
print(y.shape) # 출력 데이터의 차원: (batch_size, channels)(32, 3)
```

이를 활용하여 차원을 축소했다가 다시 차원을 확장하는 방법을 Bottleneck이라 부른다. 그대로 유지하면서 계산하는 것보다 훨씬 연산량이 적다. 하지만 없앴다가 다시 만드는 것만큼 정보의 소실이 일어나는데, 이를 nlp의 lstm과 비슷하게 이전 정보를 그대로 받아오는 방법을 사용한다면 성능이 올라갈 것이다.

여기까지 도움 받은 링크:
- [http://taewan.kim/post/cnn/](http://taewan.kim/post/cnn/)
- [https://sotudy.tistory.com/10](https://sotudy.tistory.com/10)
- [https://ariz1623.tistory.com/284](https://ariz1623.tistory.com/284)

추가로 배운 내용은 transfer learning 전이 학습이다. 이미지 같은 경우 학습량이 매우 많기 때문에 사전에 이미지를 학습시킨 모델이 있으면 이를 갖다 쓰는 것이 효율적일 것이다. 이전에 학습했던 내용이 매우 충분하다면 가중치를 업데이트 하지 않고 사용할 수 있을 것이다. 대표적으로 VGG, GoogLeNet, ResNet이 있다. 앞서 언급했던 이전 정보를 그대로 받아오는 부분은 ResNet에서 활용됨.

ResNet을 예로 들면

```python
# import
from tensorflow.keras.applications.resnet50 import ResNet50
# 사전 학습된 weights='imagenet' 사용, 맨 끝 fully connected layer 사용 안 함
resnet = ResNet50(weights='imagenet', include_top=False)
# ResNet50의 layer들은 가중치를 업데이트 하지 않을 것
for layer in resnet.layers:
    layer.trainable = False
```

그리고 dataset을 던져 주었는데...사용법이 너무 헷갈린다. 잘 익혀보아야 겠다.