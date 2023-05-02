---
title:  "AIB 딥러닝 324 Transformer(Attention)"
excerpt: "문장 내 단어의 순서를 학습하기 위해 순환하지 않고 단어 하나씩 그대로 문장 파악"
header:
  # teaser:
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, natural_language_processing, attention, transformer]
date: 2023-05-01 14:00
last_modified_at: 2023-05-01T14:00:00-09:00
---

RNN 기반은 문장 내 단어를 순차적으로 학습하는 구조이다. 이전 토큰을 학습해야 다음 토큰에 접근이 가능하기 때문에 병렬 연산은 불가능하며, 시간이 매우 오래 걸린다. 그래서 단어의 순서 정보만 별도로 보유한 채 이전 단어로부터 정보를 이어 받는 것이 아니라 단어 그 자체를 학습한다면 병렬 연산도 가능하고 시간이 훨씬 적게 걸릴 것이다. 이게 Attention만 사용한 transformer 기법이다.

Attention에는 기본적으로 인코더와 디코더가 있는데, 인코더에서 각각의 단어를 학습하고 그 결과를 디코더에서 종합해서 원하는 결과를 낸다. 구체적으로는 디코더의 히든 벡터 query와 인코더의 히든 벡터 key의 내적을 구해 softmax에 넣고 그 결과를 value 벡터에 스칼라로 곱해준다. 이 결과 벡터들을 모두 더하는데, 쿼리와 연관성이 더 높은 key에 대한 벡터의 성분이 더 많이 들어간다. query와 concat시켜 출력 단어를 결정하게 된다.

여기서 query는 내가 찾고자 하는 검색 키워드, key는 검색 결과 제목/위치/큰 부분 등으로 생각할 수 있고, value는 그 중에서 가장 알맞는 부분이다. [설명](https://stats.stackexchange.com/a/431923)

*Attention Is All You Need*에 등장한 Transformer 기준으로 정리해보려 한다.

인코더와 디코더가 있는데, 인코더와 디코더 직전에는 positional encoding, 그 전에는 각각의 embedding이 있다. positional encoding은 단어를 병렬적으로 학습할 때 이 단어가 어느 위치에 오는지에 대한 정보를 주는 부분이고, 벡터 형식으로 모델링하기 위해 embedding이 있다.

그리고 실제 인코더와 디코더는 다음으로 구성되어 있다:
- 인코더: multi-head (self) attention, feed-forward
- 디코더: multi-head (masked, self) attention, multi-head(encoder-decoder) attention, fead-forward

multi-head attention은 여러 개의 self-attention을 동시에 실행하는 것이다. 만약 같은 내용에 대해 self-attention이 있다면 앙상블 효과를 볼 수 있을 것인데, 고도화된 transformer에서는 여러 다른 부분에 대해 self-attention을 진행해서 성능을 더 높인다고 한다.

self-attention은 이름에서부터 자기 자신과 연관된 것을 의미한다. 문장 내의 단어가 문장 내에서 어떤 요소로서 존재하는지를 학습하는 것인데, 자기 자신에 대해 학습하는 것이기 때문에 self-attention이라는 용어를 사용하는 것이다.

feed-forward은 기본적으로 relu를 활성화함수로 사용한다. 여기서는 은닉층의 노드 수를 늘렸다가 다시 원래 개수로 줄여주게 된다고 한다.

masked attention은 decoder에서 사용한다. 단어들을 왼쪽부터 시작해서 오른쪽 끝까지 나열해서 문장을 구사할텐데, 그 다음에 올 단어를 가리는 식으로 그 순서를 정하게 된다. 가린다는 의미의 masked이다.

encoder-decoder attention은 encoder와 decoder의 몇몇 부분의 출력을 query, key, value로 사용하게 된다. query는 masked attention의 출력, key와 value는 encoder의 key와 value를 사용한다.

디코더의 최종 출력에서는 linear layer와 softmax layer를 거쳐서 최종 결과를 낸다는데, linear layer는 아마 가중치를 곱해서 $ \vec{y} =  W \vec{x} + b$로 벡터를 얻는 것 같다.

인코더 디코더의 각각의 단계마다 normalization과 skip connection이 있다. normalization은 학습이 빠르고 잘 되도록 수치를 조정한다고 함. skip connection은 역전파 과정에서 정보가 소실되지 않도록 방지한다고 한다. 각각 자세히는 별도로 찾아봐야겠다.

논문에서는 encoder가 6개의 동일한 layer로 구성되어 있다고 한다.

그래서 주저리 주저리 적어 보았는데, 코드는 다음 정신 말짱할 때 다시 찾아보고 블로그 업데이트 해봐야겠다.