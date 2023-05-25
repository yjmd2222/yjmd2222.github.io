---
title:  "AIB SQL-1"
excerpt: "SQL 기초 문법"
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, databse, sql]
date: 2023-05-22 11:00
last_modified_at: 2023-05-22T11:00:00-09:00
---

데이터를 전산화하여 저장할 때 데이터베이스를 사용한다. MySQL이 그 종류 중 하나이다. 데이터 관리에는 SQL 기능을 사용한다. 세부적으로는 SQL과 non-SQL로 데이터베이스를 관리할 수 있다.

직접적으로 SQL query를 사용하여 database를 관리할 수 있고, 또는 developing language로 ORM을 사용하면 SQL로 변환하여 사용할 수 있다.

데이터베이스의 필요성: 데이터는 동일한 형태로 계속 보관되어 있어야 한다. Python script는 실행중/runtime이 활성화가 되어있지 않으면 작업중인 데이터는 사라진다. 물론 중간중간 export/read할 수 있지만, 번거롭고 관리가 어렵고, 용량이 크면 시간도 오래 걸린다. 사용할 데이터셋이 많을수록 여러 변수/파일로 관리하기 힘들다. 관계형 데이터베이스로 하나의 데이터 시트를 데이터베이스 상 하나의 테이블로 관리할 수 있어서, 하나의 데이터베이스 환경에서 여러 테이블을 관리하기 수월하다.

SQL: Structured Query Language. 여러 SQL 데이터베이스에서 사용할 수 있는 언어이다. Query(질문)를 통해 구조화된 데이터베이스상 원하는 일부만 가져와 사용할 수 있다. 구조화되어있지 않다면 non-SQL이라고 한다. 예를 들어 문서 지향 데이터베이스.

SQL에는 다양한 문법이 존재하여 이로 데이터를 조회하거나 테이블을 만들 수 있다. Data Defnition Language, Data Manipulation Language, Data Control Language(접근 권한), Data Query Language, Transaction Control Language(데이터 변경사항 수정)

관계형 데이터베이스: 테이블 간의 상호작용인 Relation으로부터 관계형이라는 이름이 붙었다.
- 키워드
  - 데이터: 각 항목에 저장되는 값
  - 테이블: 행(record, tuple)과 열(column, field)로 구조화된 데이터
  - 키: 각 record를 구분할 수 있는 고유값. primary key, foreign key 등이 있음.

관계는 테이블 간의 관계와 자신 안에서의 관계를 말할 수 있다.
- 테이블 간의 관계
  - 1:1; 하나의 테이블의 record에 고유한 foreign key가 있어서 테이블이 1:1 매칭이 된다. 하나로 병합 가능함. 그래서 애초에 병합해서 사용하는 경우가 많아 찾아보기 어렵다.
    - 실무: 중복 걸러주는 용도로 사용 가능 또는 여러 테이블이 있을 때 그 테이블에 공통되는 정보 저장
  - 1:N; 하나의 테이블의 record가 다른 테이블의 여러 records와 연결이 가능하다. 즉 여러 foreign key가 하나의 primary key를 가질 수 있는 것.
  - N:N; 하나의 테이블의 records가 다른 테이블의 여러 records와 관계를 가진다. 이를 분석하면 1:N 두 개의 관계로 나눌 수 있다. (1:N, N:1) 중간 테이블: join table.
    - 중복 제거 관련해서 many-to-many로 저장해야 사용하기 편하다. 만약 합쳐서 관리한다면 다 읽은 다음에 DISTINCT/AGGREGATE로 뽑아와야 하기 때문에 성능이 떨어진다.
  - 자기참조: 하나의 테이블의 record는 하나의 foreign key를 가지는데, 이 foreign key가 고유하지 않은 경우. 1:N 관계를 하나의 테이블로 병합한 것으로 생각해볼 수 있다.

스키마: 데이터가 구성되는 방식과 서로 다른 엔티티(테이블) 간의 관계에 대한 설명, 즉 데이터베이스의 청사진. 위 관계를 설명하는 것

연관관계가 많다면 관리하기 어려울 수도 있다. 뛰어난 개발자는 FK를 안 쓴다고 한다.

SQL tip: `테이블.field`로 표기 가능. 테이블 이름이 길다면 `AS`로 alias를 설정해서 사용 가능.

`NULL`: 결측치가 존재할 수 없다. 주로 `Primary Key`에 해당한다.

`PK`: `Primary Key`

SQL에 index를 태운 것(메모리에 탑재)과 안 태운 것의 query할 때 full scanning하는 것과 index만 가져오는 것에 엄청 차이 난다.

SQL 데이터베이스마다 사용할 수 있는 query가 다르다. 오늘 과제 중에 FK를 설정해야 하는 문제에서 설정 안 하고 나중에 추가하려고 보니 SQLite에서는 추가가 안 된다. 테이블 새로 생성해야 한다.

SQLInjection: constant를 명령어로 넣는다면 해킹 가능. SQLite3에서는 prepared Statement이 있어서 그런 부분을 제거 가능하다고 함.

## 기타
- JOIN은 전체 테이블을 조회해야 하기 때문에 where가 좋을 수도 있다.
- OUTER JOIN은 데이터 삭제에 응용될 수 있다.
- LIMIT은 얼마 만큼의 데이터를 보여줄 때 OFFSET과 같이 사용할 수 있다.
  - OFFSET은 위에서부터 계산해야 하기 때문에 뒤로 갈수록 느려질 수 있다.
- SQL VS. no SQL 장단점이 있음.

퀴즈: https://sqlbolt.com/lesson/
튜토리얼: https://www.w3schools.com/sql/