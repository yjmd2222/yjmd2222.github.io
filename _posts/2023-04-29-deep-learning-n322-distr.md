---
title:  "AIB 딥러닝 322 분산/분포 표현"
excerpt: "토큰을 임베딩해보기"
header:
  teaser: /assets/images/s3dl22-distr.webp
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, natural_language_processing, word2vec, distributed_representation]
date: 2023-04-29 19:00
last_modified_at: 2023-04-29T19:00:00-09:00
---

지난 시간에 단어의 등장횟수 기반으로 문서를 벡터화 했었다. 이번에는 문서에서 단어의 분포(분산)를 기반으로 문서를 벡터화해본다.

분산 표현은 분포 가설을 기반으로 한다: "비슷한 위치에 있는 단어는 비슷한 의미를 가진다." 단어 하나만 빼 놓고 문장을 본다면 "나는 지금 쓰고 있는 스마트폰의 성능이 느리지 않고 __"라고 했을 때 '좋다'나, '괜찮다'를 빈칸에 넣으면 비슷한 의미의 문장이 될 것이다.

단어를 선택해서 문장에 집어넣을 때 단어를 어떻게 벡터화해야 할지 고민해보아야 한다. 일반적으로 자주 사용하는 원-핫 인코딩을 여기에도 적용하면 아무리 비슷한 단어라도 서로 다르다면 단어 간 유사도를 확인할 때 내적하면 0이 나올 것이다. 그래서 **임베딩**을 한다.

원-핫은 각 element가 이진 값을 가진다면 임베딩은 **연속적인 값**을 가진다. 그래서 서로 다른 단어라도 내적하면 0이 나오지 않을 수 있게 된다:

```python
oh_vec_1 = [0, 1, 0, 0, 0]
emb_vec_1 = [0.2, 0.5, 0.2, 0.1, 0.1]
```

임베딩을 잘 수행하는 방법으로는 **Word2Vec**이 있다. 세부적으로는 Continuous Bag-of-Words(CBoW)와 skip-gram이 있다. **CBoW**는 단어 한 자리를 비워놓고 주변 단어 정보를 가지고 비워둔 중심 단어의 정보를 예측하고, **skip-gram**은 단어 하나만 남겨놓고 주변 단어의 정보를 예측한다. 중심 단어로부터 `window_size`만큼 좌 그리고 우를 살핀다. 그래서 `window_size=2`에 대한 *총 윈도우 크기*는 5가 된다. 역전파로부터 가중치 갱신이 더 많이 되는 skip-gram이 성능이 약간 더 좋다고 한다. 그리고 다른 모든 단어를 예측해야 되기 때문에 리소스를 더 많이 잡아먹는다고 한다.

CBoW와 skip-gram 둘 다 학습하는데 오랜 시간이 걸린다고 한다. 그래서 negative-sampling과 subsampling로 단축할 수 있는데, negative는 상관 업는 단어에 대해서는 몇 개만 sampling하는 방식으로 학습하고, subsampling은 쓸모 없는 단어에 대해 확률적으로 거르는 방식으로 sampling하여 학습한다.

이렇게 학습된 결과에 대해 말할 것이 있는데, 임베딩은 단어의 **의미의 유사성** 뿐만 아니라 **구조적 유사성**도 나타낸다. 예를 들어서 male과 female, boy와 girl이 벡터스페이스에서 유사한 관계를 나타내는 것을 확인할 수 있다.

<table>
  <tr>
    <td style='text-align:center;border-bottom:none;'>
        <img src="/assets/images/s3dl22-distr.webp">
        <a href="https://towardsdatascience.com/word2vec-research-paper-explained-205cb7eecc30">
        https://towardsdatascience.com/word2vec-research-paper-explained-205cb7eecc30
      </a><img>
    </td>
  </tr>
</table>

그리고 전에 보지 못했던 **out-of-vocab**를 풀고자 하는 문제에 적용할 수 없다. 그래서 **fastText** 기법이 있는데, 단어에서 **철자** 하나를 토큰으로 생각해서 임베딩. 이렇게 하면 비슷한 단어끼리 유사도를 볼 수 있다.

강의 노트에서는 CBoW, skip-gram, 그리고 fastText를 직접 해보진 않았다. [CBoW를 직접 진행하는 코드](https://www.kdnuggets.com/2018/04/implementing-deep-learning-methods-feature-engineering-text-data-cbow.html)

Word2Vec이 적용된 결과를 사용해보자 할 때 **gensim** 패키지를 이용할 수 있다: [튜토리얼](https://radimrehurek.com/gensim/auto_examples/tutorials/run_word2vec.html)

추가로 강의노트에서 imdb 데이터셋을 로드해보았는데, tf keras에서 IMDB movie review 데이터셋을 제공해준다.
```python
# 아래 load_data method를 보면
# index 1은 start_char, 2는 oov, 3부터가 실제 단어들이다.
tf.keras.datasets.imdb.load_data(
    path="imdb.npz",
    num_words=None,
    skip_top=0,
    maxlen=None,
    seed=113,
    start_char=1,
    oov_char=2,
    index_from=3,
    **kwargs
)
```

```python
# imdb의 기존 index를 호출. 여기서는 start_char, oov를 포함하지 않고 0부터 시작.
word_index = imdb.get_word_index()

# start_char과 oov를 고려해서 index에 +3 해준다.
reverse_word_index = {value + 3: key for key, value in word_index.items()}

# start_char=1, oov_char=2를 고려하여 1 -> <START>, 2 -> <OOV>로 저장합니다.
for i, word in enumerate(['<START>', '<OOV>']):
    reverse_word_index[i+1] = word

def decode_review(text):
    return ' '.join([reverse_word_index.get(i, '?') for i in text])

(X_train, y_train), (X_test, y_test) = imdb.load_data(num_words=20000)
decode_review(X_train[0])
```
이런 식으로 imdb를 decode할 수 있다.

`pad_sequences`라는 것이 있는데, `tokenizer.fit_on_texts(sent)`를 하면 `tokenizer.texts_to_sequences`으로 그 texts/sentences에서 등장한 단어들에 부여된 숫자를 nested array로 받아올 수 있다. 이를 차원을 맞춰주는게 `pad_sequences`. `padding` 파라미터에 어느 값을 넣느냐에 따라 반환되는 형태가 다른데, `'post'`는 inner array 끝에 padding한다. `from tensorflow.keras.preprocessing.sequence import pad_sequences`나 `from tensorflow.keras.utils import pad_sequences`로 불러온다. padding을 하느냐 안 하느냐에 따라 [*shuffle*](https://www.quora.com/Which-effect-does-sequence-padding-have-on-the-training-of-a-neural-network)이 다르게 적용되는 것 같다.