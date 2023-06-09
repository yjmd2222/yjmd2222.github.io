---
title:  "AIB 딥러닝 312 인공신경망 학습"
excerpt: "인공신경망 학습의 기본 요소들에 대해 배웠다"
header:
  teaser: /assets/images/pizza.webp
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, neural_network, gradient_descent]
date: 2023-04-18 19:00
last_modified_at: 2023-04-18T19:00:00-09:00
---

어제는 단순하게 딥러닝을 돌려보기만 했었다. 오늘은 딥러닝의 학습과정에 대해 배웠다.

입력층에 입력값이 들어오고 은닉층에 가중치 연산 후 출력층에 출력값이 반환된다. 은닉층에서 순전파로 가중치가 연산되고, 출력층의 예측값을 손실함수로 손실 정도를 줄일 수 있는지 역전파로 가중치를 갱신한다. 이를 계속 반복하는 것이다. 한 번 전체 데이터에 대해 진행하면 그것을 1 epoch라 부른다.

역전파에서 가중치가 업데이트 되는 방식은 경사하강법이나 파생법으로 손실을 줄여보는 것. 수학적으로 매우 복잡하지만, 현재로서는 이론학자가 되려는게 아니기 때문에 다음의 하나만 기억한다: $ w_{i,j+1} = w_{i,j} - \eta \frac {\partial J(\vec{w_j})} {\partial w_{i,j}} $. 학습률 $ \eta $를 손실 정도 $ J' $과 곱해 그 다음 $ j+1 $번째로 넘어가면서 최소점을 찾는 것. 어느 optimizer를 사용하느냐에 따라 $ J' $의 계산 방식이 달라질 것이다. 전체 데이터에 대해 계산해서 한 번 움직이는 GD, 데이터포인트 하나에 대해 계산해서 움직이는 Stochastic GD, batch로 나누어서 계산하고 움직이는 mini-batch, 그리고 가장 빠르면서 성능이 괜찮은 ADAM이 있다.

GD는 전체를 계산하고 움직이기 때문에, 한 번 움직일 때까지 시간이 오래 걸린다. SGD는 하나에 대해 계산하고 움직이기 때문에, 몇 번 움직여서 최소점에 도달하면 빨리 끝날 수 있다. ADAM은 정확히 어떻게 돌아가는지 모르겠지만 자주 사용할 것 같음.

아무튼 이렇게 여러 번 epoch를 진행할텐데, 그때마다 랜덤하게 데이터 순서를 바꾸는 `.shuffle()`이 자동적으로 적용되어 일반화 성능이 올라갈 것이다.

가중치와 편향은 다음 노드로 넘어갈 때 계산에 사용되는 값이라고 생각한다. 이진분류에서 가중치를 곱했을 때 0.5보다 크면 sigmoid로 1을 만들고 작으면 0을 만들 수 있다. 편향은 아직 자세히 안 배운 것 같음.

그리고 은닉층 개수를 더하면 더할수록 gradient가 사라질 수 있다고 하는데, 숫자가 아닌 함수로 기억하고 있으면 그 걱정은 없을 것 같다는 생각이 들었지만, 다 기억하려면 메모리가 어느 정도 들지 모르겠다.

오늘 딥러닝 코드는 어제와 다를게 없다. 순전과 역전을 직접 구현하는 코드를 만들어서 순전과 역전에 대해 자세히 배웠지만, 이 코드는 실사용에 필요 없다. 그런데 오늘 과제 중에 `Dense()` 안에 `input_dim` kwarg를 봤는데, 강의 노트나 영상에서 이걸 본 기억이 없다. 경우에 따라서 `input_shape`이나 `input_dim`을 사용하는데, 다음 층에 넘길 노드 개수라고 생각하면 될 것 같다.

피자 먹고 싶어서 피자 사진 올린다. 

<p align="center">
  <img src="/assets/images/pizza.webp" />
  <center>https://www.myrecipes.com/recipe/pepperoni-pizza</center>
</p>