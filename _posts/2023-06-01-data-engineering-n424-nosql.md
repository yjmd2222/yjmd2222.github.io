---
title:  "AIB 데이터 엔지니어링 424 NoSQL"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, nosql]
date: 2023-06-01 09:00
last_modified_at: 2023-06-01T09:00:00-09:00
---

- 정형, 비정형, 반정형

컴퓨터의 소통은 구조화된 정보를 텍스트로 나타낼 수 있어야 한다. xml은 텍스트 정보를 tag로 구분해서 나타냄. json은 tag을 열고 닫는 것이 없어서 더 간편하지만 오히려 문법 오류 취약. yaml은 사람 볼 수 있도록 만듦: indent와 줄바꿈 필수.

NoSQL: 관계형 데이터 모델을 사용하지 않는 DB 형식. 웹 시장의 발전에 따라 폭발적으로 발생하는 데이터에 대해 이를 다루기 위해 많이 사용되었다. xml이나 json의 처리를 RDB로 하려면 파싱한 후에 넣어야 하기 때문에 시간이 오래 걸리고, 여러 서버를 연결하는 것이 비용 절감됨. 여러 NoSQL DB끼리 공통점이 많이 없다.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|SQL|NoSQL
-|-|-
데이터 저장|스키마에 따라 데이터를 테이블에 저장|여러 형식으로 저장: k-v, document, graph, wide-column
스키마|형식이 고정된 스키마 필요. 이를 변경하게 되면 전체 데이터베이스가 영향을 받는다.|동적 스키마. 행을 추가할 때 열도 추가하거나, 개별 속성에 대해 모든 열에 대해 데이터 입력하지 않아도 된다.
쿼리|필요한 정보 요청하기 위해 테이블간의 관계에 맞추어서 쿼리를 보낸다.|데이터 자체를 조회하는 것에 초점을 맞춤
확장성|수직적으로 확장: 메모리, CPU 등 하드웨어 성능이 필수.|수평적으로 확장: 클러스터링 - 추가 서버 구축하면 되고, 비교적 저사양 하드웨어로 NoSQL DB 호스팅 가능.
사용 케이스|ACID 성질 준수 필요. 전자거래, 금융서비스 등. 또한 규모가 작거나 일관된 데이터를 사용하는 경우.|데이터 구조가 거의 없고 대용량의 데이터를 저장하는 경우 / 클라우드 기반 DB라면 번거로움 없이 확장할 수 있는 NoSQL이 편리함 / 데이터 구조를 자주 업데이트 하는 경우

NoSQL 데이터 저장 형식
- k-v: 속성 이름과 속성에 연결돈 값. 대표 DB: Redis, Dynamo
- document: 데이터를 문서 하나로 저장하는 것. 문서 하나마다 속성 하나가 있고, 컬렉션으로 묶어서 관리한다. MongoDB
- wide-column: 열에 대해 집중해서 관리함. 각 열에 k-v 형식으로 데이터를 저장하고 컬럼 패밀리로 열의 집합 단위로 데이터를 처리한다. 하나의 행에 여러 열이 있어서 높은 유연성이 있고, 규모 큰 데이터 분석에 주로 사용된다. Cassandra, Hbase

Document database

MongoDB를 RDB와 비교하면 테이블은 컬렉션, 행은 Document, 열은 Field이다. 데이터 타입에서 자유롭고, 많은 양의 데이터를 관계 상관 없이 추가할 때 사용된다. 스키마가 있어야 데이터를 수월하게 조회할 수 있다. RDB는 스키마를 사전 정의하고 데이터를 적재했었는데, MongoDB는 데이터를 읽을 때 특정 스키마를 따라야 할 수도 있다.

[튜토리얼](https://pymongo.readthedocs.io/en/stable/tutorial.html) 좀 더 살펴봐야 겠음.

SQL의 `DROP TABLE IF EXISTS table_name`은 익숙한데, NoSQL에서는 잘 몰라서 자꾸 과제에서 에러가 났었다. `try: mongo_client.drop_database('unwanted_database')` 이런 형식으로 드롭 가능한 것 같다.