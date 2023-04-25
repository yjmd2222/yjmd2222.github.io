---
title:  "AIB 딥러닝 321 자연어처리"
excerpt: "자연어를 이용한 결과를 만들기 위해 기본적으로 어떤 것을 해야 하는지 배워보자"
header:
  teaser: /assets/images/s3dl21-nlp.png
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning, 'natural_language_processing', 'nlp']
date: 2023-04-24 19:00
last_modified_at: 2023-04-25T19:00:00-09:00
---

자연어를 이용한 인공지능 결과들이 여럿 있다.
- 자연어 이해: 글의 내용이나 제목에 따라 분류, 어떤 가설이 있을 때 맞는지 틀리는지의 추론, 독해, 질의응답, 문장 내 단어에 대한 품사 태깅, 문장 내 단어를 종류로 분류하는 개체명 인식
- 자연어 생성: 글 작성
- 복합: 번역, 요약, 챗봇, TTS, STT, 이미지 캡션생성 등이 있다.

이걸 다 배워볼 여력은 없겠다. 내가 풀고자 하는 문제가 어느 범주에 속하는지 파악해서 그 하나를 파고들 것.

컴퓨터는 자연어를 그대로 인식할 수 없기 때문에 인식할 수 있도록 바꾸어주어야 한다. 토큰화로 의미 있는 단위의 토큰 (주로 단어)을 구분한다. 그리고 횟수 기반으로 의미를 부여해 입력하는 등장 횟수 기반 표현 (Count-based Representation: Bag-of-Words, Term Frequency-Inverse Document Frequency)과 중요한 단어를 기준으로 주위의 분포를 같이 고려하는 분포 기반 표현 (Distributed Representation: Word2Vec, GloVe, fastText)이 있다. 오늘은 횟수 기반 위주로 배웠다.

이렇게 인식을 시키더라도 문장 형태가 조금만 바뀌면 특성이 하나 더 늘어나게 된다. 대소문자, 마침표, 단어 형태 등. 그리고 너무 자주 사용하거나 거의 사용하지 않는 단어, 또는 의미 없는 불용어들은 제거해주는게 좋을 것이다.

이렇게 처리하고 단어도 컴퓨터가 인식할 수 있도록 데이터프레임 형태 등으로 바꾸어주면 이제서야 무언가를 해볼 수 있다. 오늘 노트에서는 `Nearest_Neighbor`로 유사한 문서가 무엇인지 찾아보는 것을 해보았고, 퀴즈에서는 전처리로 얻은 데이터프레임과 중요 키워드 하나가 존재하는지 아닌지 이진분류 문제로 접근해보았다.

자주 사용하게 될 모듈로 spaCy와 nltk가 있다. spaCy가 조금 더 빠르다고 하는데 stemming 기능이 아직 구현되지 않았다고 한다.

아래 코드와 같이 짚고 넘어갈 점을 정리해보고자 한다.

```python
import spacy
from spacy.tokenizer import Tokenizer

nlp = spacy.load("en_core_web_sm") # en: 영어
#
# 거의 여기까지가 기본 구조. 무엇을 하고자 하느냐에 따라 다른 것 같다.
#

# vocab은 언어 모델의 어휘 집합을 불러올 때 사용합니다.
# spacy의 토크나이저에 영어 어휘 집합을 넣어 영어를 토큰화해주는 토크나이저를 생성합니다.
tokenizer = Tokenizer(nlp.vocab)
```

```python
def apply_lowercase(sent):
  return sent.lower()

def sub_alnumlower(sent):
  return re.sub(r"[^a-z0-9 ]", "", sent) # escape character 처리하려면 더 복잡하게 구성해야 함

def my_func_1(col):
  col_tokens_prep = []
  for prep_sent in tokenizer.pipe(col):
    sent_token_prep = [sub_alnumlower(apply_lowercase(token.text)) for token in prep_sent]
    sent_token_prep = [text for text in sent_token_prep if text != '']
    col_tokens_prep.append(sent_token_prep)
  return col_tokens_prep

tokens = my_func_1(df['description'])
```

디스코드에서 동기들도 그렇고 regex 사용에서 많은 애를 먹었다. 강의노트 자료는 대부분 escape character가 없어서 별도 처리 없이 대소문자 처리와 특수문자 처리만 했는데, 문제점도 있다. 특수문자를 공백으로 치환하면 콤마나 마침표만 사라지는게 아니라 - 등이 사라져서 단어가 붙어 버리는 경우도 생긴다. 그래서 regex를 이용해서 특수문자와 escape character를 잘 처리할 수 있으면 좋을텐데, 아직 어떻게 할지 생각은 잘 안 난다. 대신 (좀 더 자세히 찾아봐야 겠지만)먼저 `tokenizer`에 입력하면 적절히 공백으로 처리해주는 것 같다. `tokenizer.pipe`에 iterable('str') 형태로 입력하는 것이 한 가지 방법이다.

`collections`의 `Counter`로 단어 등장 횟수를 파악할 수 있다.

```python
from collections import Counter

word_counts = Counter() # 새롭게 집계하고 싶으면 새로 선언

# .update는 기존에 대해 update하게 된다. 각 데이터포인트별로 .update 하는 것을 보면 쉽게 알 수 있다.
df['tokens'].apply(lambda x: word_counts.update(x))

word_counts.most_common(10)
```

이를 이용해서 횟수를 셌을 때 `squarify` 모듈로 시각화해볼 수 있다.

```python
!pip install squarify

import squarify
import matplotlib.pyplot as plt

wc_top20 = wc[wc['rank'] <= 20]
squarify.plot(sizes=wc_top20['percent'], label=wc_top20['word'], alpha=0.6)
plt.axis('off')
plt.show()
```

spacy에서 디폴트로 기록해둔 불용어에 더하거나 빼서 나만의 불용어 집합을 만들 수 있다.

```python
print(nlp.Defaults.stop_words) # spacy.load('en_core_web_sm').Defaults.stop_words

stop_words = nlp.Defaults.stop_words.union(words_to_add) # 추가
stop_words.difference_update(words_to_remove) # 제거
print(stop_words)
```

Trim: 자주 등장하는 단어와 거의 없는 단어 제거하기. 단어별로 등장 비율을 구한다면 indexing해볼 수 있을 것이다. 문서에 어느 정도 비율로 등장하는지, 또는 문서별로 몇 번 이상 등장했을 때 전체 문서에 대한 비율로 생각해볼 수 있다. 뭐가 더 나은지는 모르겠지만, 강의노트는 후자를 다룬다. indexing을 완료했다면 tokens에서 제거하면 될 것이다.

```python
indexed_words = wc[0.9 > wc['percentage_of_docs'] > 0.01]
df['trimmed'] = df['tokens'].map(lambda x: [w for w in x if w in indexed_words])
```

단어 형태 조정에는 어간추출(stemming)과 표제어추출(lemmatization)이 있다. spaCy에는 어간추출이 없어서, 강의노트는 nltk를 이용한 stemming을 다룬다.

```python
from nltk.stem import PorterStemmer

ps = PorterStemmer()

tokens = []
for doc in df['tokens']:
    doc_tokens = []
    for token in doc:
        doc_tokens.append(ps.stem(token))
    tokens.append(doc_tokens)

df['stems'] = tokens
```

Lemmatization은 Spacy를 이용한다. nltk보다 빠르다고 하는데, 여전히 느리다. 구글에 보면 parallel computing인지 pandarallel를 이용하면 엄청 빠르다고 하는데, 코랩이라서 그런지 거의 차이가 없었다.

```python
!pip install pandarallel
from pandarallel import pandarallel
pandarallel.initialize(progress_bar=True) # 진행도

def get_lemmas(tokens):

    lemmas = []
    
    doc = nlp(' '.join(tokens))

    for token in doc:
        # pos_ : 해당 토큰의 품사(part of speech). 'PRON'은 대명사를 의미
        if token.pos_ != 'PRON': lemmas.append(token.lemma_)
    
    return lemmas

df['lemmas'] = df['tokens'].map(get_lemmas)
```

여기까지가 전처리라고 볼 수 있고, 사용하기 위한 데이터프레임 반환을 위해 벡터화를 진행해야 한다. Bag-of-Words는 단어 빈도가 큰 것을 중요하게 삼아서 수치를 부여하고, TF-IDF는 단어 빈도가 작은 것을 중요하게 삼아서 수치를 부여한다.

```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

# spacy.load()에 알맞은 형태로 넣기
df['sent_lemmas'] = df['lemmas'].map(' '.join)

# TF-IDF
tfidf = TfidfVectorizer(max_features=20) # 몇 개의 토큰을 사용할지 횟수 순으로
tfdtm = tfidf.fit_transform(df['sent_lemmas'])
tfdtm = pd.DataFrame(tfdtm.todense(), columns=tfidf.get_feature_names_out())
display(tfdtm)

# BoW
bow = CountVectorizer()
bwdtm = bow.fit_transform(df['sent_lemmas'])
bwdtm = pd.DataFrame(bwdtm.todense(), columns=bwdtn.get_feature_names_out()) # csr_matrix 연산 속도 관련해서 빠르게 처리한 것을 todense로 df에 알맞은 형태로 변환
display(bwdtm)
```

벡터화했다면 이를 비교하는 방법 또한 여러 가지이다. 코싸인 내적을 하는 방법도 있고, 거리로 환산해서 가까운지를 확인할 수도 있을 것이다. 강의노트에서는 KNN을 사용한다.

```python
from sklearn.neighbors import NearestNeighbors
nn = NearestNeighbors(n_neighbors=5, algorithm='kd_tree') # 갯수와 계산 알고리즘
nn.fit(tfdtm)

nn.kneighbors([tfdtm.iloc[index]]) # 몇 번 index와 유사한 것들이 있는지 확인
```

강의 노트에 PCA import도 봤는데, 단어가 너무 많다 보니 차원 축소하는 방법도 적용해볼 수 있겠다.