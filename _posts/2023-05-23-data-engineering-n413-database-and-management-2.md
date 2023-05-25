---
title:  "AIB SQL-2"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, databse, sql, transaction]
date: 2023-05-23 11:00
last_modified_at: 2023-05-23T11:00:00-09:00
---

### 키워드
- Transaction, ACID

DBMS: DB를 관리하는 기능; 이 과정에서 추가적인 데이터가 생성된다. 이를 메타데이터/카탈로그라 부른다. 예를 들어 데이터 유형, 구조, 인덱스, 사용자 그룹 등이 있다.

DB system: DB, DBMS, 관련된 application을 묶어서 부른다.

SQL Query를 DBMS에서 확인하고 metadata를 확인하고 요청받은 데이터를 app에 돌려준다.

### 트랜잭션 Transaction

트랜잭션: 데이터베이스와 관련된 일련의 작업들에 대한 연속처리 단위를 의미한다. 즉 데이터베이스 상태를 변화시키는 작업의 모음이다.

commit: 메모리에 있는 변경 예정 사항을 데이터베이스에 반영

roll-back: 메모리에 있는 변경 예정 사항을 반영하지 않고 취소

DBeaver는 default가 트랜잭션에 auto-commit을 수행한다. 이를 해제하고 `COMMIT;`을 입력할 수 있다.

강의 노트에서 terminal에 python 실행하길래 그대로 해보았더니 다음 줄로 넘어가고 가만히만 있다. 이런거 설명좀 해줬으면 좋겠지만 기대를 버리고 직접 찾아보면 git bash는 winpty가 아니면 뭔가 실행이 잘 안 된다. `alias python='winpty python.exe'`로 `python`을 입력하면 `winpty python.exe`가 실행되도록 source를 설정해주면 된다: `echo "alias python='winpty python.exe'" >> ~/.bashrc`. [참고](https://stackoverflow.com/a/36530750)

### ACID

ACID: atomicity, consistency, isolation, durability. 안전성

Atomicity: transaction은 전부 실행 또는 전부 실패. All or nothing. 중간까지 실행하거나 중간부터 실행하고 나머지는 실패하는 현상은 있을 수 없다.

Consistency: 데이터베이스 상태는 일관적이어야 한다. transaction 전과 후가 이전과 같이 유효해야 한다. 유효하기 위한 규칙과 제약을 위반할 수 없다. 예를 들어 PK는 모든 행에 대해 존재해야 하는데 이 값을 삭제하는 transaction은 실행될 수 없다.

Isolation: 각각의 transaction은 서로 독립적이어야 한다. 두 개의 transaction이 동시에 일어난다고 연속적으로 일어난 것과 마찬가지인 결과를 내야 한다.

Durability: transaction의 실행과 오류에 영구적으로 로그가 저장되어야 한다.

### More SQL
- GROUP BY, HAVING, COUNT(), SUM(), AVG(), MAX(), MIN()

HAVING: GROUP BY로 조회된 결과에 대한 필터. GROUP BY가 아닌 상황에서는 WHERE를 사용해야 한다.

실행 순서: FROM &rarr; WHERE &rarr; GROUP BY &rarr; HAVING &rarr; SELECT &rarr; ORDER BY 등; JOIN이나 GROUP BY할 때 SELECT로 기준 field를 뽑아내지 않아도 잘 진행됬던 것이 가능했던 이유.

CASE: python match case와는 다르다. CASE는 SELECT CASE에 사용하고, if 대신 WHEN을 사용한다. elif도 아니고 다시 WHEN
```sql
SELECT customerId, CASE
			WHEN CustomerId <= 25 THEN 'GROUP 1'
			WHEN CustomerId <= 50 THEN 'GROUP 2'
			ELSE 'GROUP 3'
		END
	FROM customers
```

SUBQUERY: 쿼리문 안에 다른 쿼리문 작성하는 것. 괄호 `()`로 감싼다. FROM이나 IN 등의 여러 상황에서 사용할 수 있다.

GROUP BY 
1. 어떤 칼럼(KEY)을 기준으로 분할
2. 기준 칼럼에서 보고싶은 집계 연산을 적용한 다른 칼럼이 필요

###

실무에서 SQL은 대부분 API 이용하는데, transaction annotation을 걸어주게 된다.

Transaction을 pseudo code로 나타내면
```
transaction
	lock()
	do_something1()
	do_something2()
	unlock()
```

transaction은 병렬 데이터 이점이 없을 수 있다. 병렬 이점을 사용하기 위해 NoSQL 사용.

MSA vs. Monolithic
- MSA
	- 여러 기능이 하나 안에 들어있는 것이 아니라 분산되어 있는데, 그 기능들 중 하나가 오류가 나면 transaction 추적이 어렵다.
	- 각각 기능을 다른 언어로 만들 수 있다. 서로 통신할 프로토콜은 잘 정해주어야 한다.
- Monolithic은 하나의 기능은 traffic이 작지만 다른 기능이 traffic이 많을 때 서비스가 분리가 되어 있지 않아서 통째에 대해 backup이나 증설을 해야 한다.

CS 잘 공부해보자. 면접에 CS 질문 가득. 자료도 찾아보고 직접 문제 풀어보기