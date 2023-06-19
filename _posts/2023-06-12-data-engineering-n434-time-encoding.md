---
title:  "AIB 데이터 엔지니어링 434 시간과 부호화"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, time, encoding]
date: 2023-06-12 09:00
last_modified_at: 2023-06-12T09:00:00-09:00
---

- [x] csv, json
- [ ] 데이터 APScheduler로 저장
- [x] Parquet, Avro
- [x] 데이터 압축 필요한 상황
- [x] 스트리밍형과 배치 형 데이터 플로우와의 연관성 중 스케줄러와 시간의 중요성

unicode: 문자를 하나(uni)의 문자 셋에 포함시킴. 문자의 이진수를 인코딩하는 방식이 다를 수 있다. 그 중 하나 utf-8. url encoding은 입력한 글자를 ascii로 치환.

마주칠 수 있는 문제점들. 공식 문서를 확인해서 디버깅할 수 있어야 한다.

## 시간
1. 시간을 활용하는 라이브러리나 API는 시간에 대해 알고 있어야 한다.
2. 데이터를 저장할 때 시간을 저장해야 하는 경우가 있다.
3. 여러 노드에서 시간을 발생시키는 경우?

예를 들어 Github API에서 Issue를 조회할 때 어떤 시점 이후부터 issue를 조회할 수 있다. Github API의 since encoding은 ISO-8601. Tweepy는 UTC.

터미널의 시간 git bash
- `date`: 보통 현재 컴퓨터 시간.
- `date -u`: UTC 기준으로 시간으로 나타남. 협정ㅇ 세계시. Coordinated Universal Time. 영국 시간. KST는 UTC+9
  - GMT도 비슷하다.

ISO 8601: 국제 표준 표현. 년월일시분초타임존. 연도는 4자리, 나머지 시간은 2자리. 구분자로 구분한다
  - 구분자: `-, :, Z, T` Z는 Timezone, T는 시간

Unix Time, Epoch Timestamp: 19700101T000000을 기준으로 몇 초가 흘렀는지. 시간을 알기 위해서보다는 시간 연산을 위한 표준이다. Z 없으면 UTC
  - `date +%s`

## 스케줄링
어떤 시간 기준으로 어떤 작업을 수행하고자 할 때 필요하다.

APScheduler vs. Cron: 파이썬 라이브러리, 앱 vs. unix 운영체제

APScheduler 순서: 스케줄러 선언, job 선언, 스케줄러 시작

BlockingScheduler: 스케줄러 자체가 프로그램. 나머지 스케줄러는 다른 프로그램과 연동.
- 스케줄러 선언 `sched = BlockingScheduler()`
- job 정의 후 추가 `func = lambda x: print('what')`; `sched.add_job(func, trigger='interval', seconds=5)`
- 시작 `sched.start()`

tip: VSCode에서 ctrl + shift + p 하면 interpreter 선택할 수 있다.

## 모델 저장
파이썬 대표적으로 pickle. pickling.

파이썬 오브젝트는 메모리 안에 있다. 인 메모리. 이 데이터를 밖으로 꺼내와야 한다. pickle이 이 기능을 수행한다.
```python
# 피클링
import pickle

with open('something', 'wb') as file: # write byte
  pickle.dump(something, file)
```
```python
# 역피클링
import pickle

with open('something', 'rb') as file:
  something = pickle.load(file)
```

바이트 형태로 변환한다.

WEB JSON
```python
import json

data = [{'something': 'something'}]

with open('json_file.json', 'w') as file: # 
  json.dump(data, file)
```
```python
import json

str_1 = None

with open('json_file.json', 'r') as file:
  str_1 = json.load(file)

print(str_1)
```

## 용어 정리
- 부호화: 데이터 저장/전송하기 쉽게 변환하는 과정. 인코딩, 직렬화, 피클링, 마샬링
- 복호화: 부호화된 데이터를 해석하는 과정. 디코딩, 역직렬화, 역피클링, 언마샬링

## 추가

CSV: nesting 관련해서 문제점 있음. 예를 들어 어떤 경우는 `DictReader`는 `with` 밖에서 조회 되는데 `reader`는 안 될 수도 있음. 변수 약간 변형해서 새로 지정하면 사용 가능했음.

json: json은 보통 웹이랑 연동해서 사용하는데, 자체 연산이 매우 느리기 때문에 웹 구동도 느려질 수 있다.

pickle: byte라서 decode해야지만 확인 가능. version compatibility issues across python. pickle도 기본 pickle은 느림. cPickle 빠름.

[JSON, CSV 추가 reading](https://kgw7401.tistory.com/74)

Avro, Parquet: 파일 포맷. 특별히 데이터 스키마를 포함하고 있다.
- Avro는 블록으로 구분되어 있다고 함. 하둡 맵리듀스 입력으로 효율적으로 사용될 수 있다고 하는데...
- Parquet 컬럼 기반 저장 포맷. 압축률이 매우 높다고 한다. 컬럼은 같은 데이터형일 가능성이 매우 크기 때문에.
- 데이터 압축을 통해 storage usage, data transmission time, communication bandwith 즉 비용을 줄일 수 있다. 시간적으로나 물질적으로나.
- https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=phh0606c&logNo=222182498678
- https://kgw7401.tistory.com/74

스트리밍 데이터, 배치 데이터
- 스트리밍은 실시간으로 하나씩 데이터 받은 것을 보내기
- 배치는 몇 개씩 누적했다가 주기적으로 보내기.
- 각각 보내는 주기가 다를테니 이에 맞추어 스케줄러를 짜야할 것이다.


## 오전
```python
# epoch
import time

start_time = time.time()

list_ = []
for i in range(10000):
  list_.append(i)

end_time = time.time()
print(start_time)
print(end_time)
print(end_time - start_time)
```
```python
# epoch to localtime
import time

epoch_time = time.time()
print(epoch_time)

date = time.localtime(epoch_time)
print(date)
```
```python
# read time from str
import time
from datetime import datetime

date = '2019-09-01'
new_date = datetime.strptime(date, '%Y-%m-%d')

print(new_date)
```

## 오후
- OSI 7계층: 네트워크를 층으로 나누어놓음.
- wandb, jira, trello