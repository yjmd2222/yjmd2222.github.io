---
title:  "AIB Application Programming Interface (+알고리즘)"
excerpt: "API"
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, api, application_programming_interface]
date: 2023-05-24 09:00
last_modified_at: 2023-05-24T09:00:00-09:00
---

- Interface: 두 개를 연결시켜주는 것.
- PEP 249: DBAPI v2.0 기준문서. API가 이를 바탕으로 작성되기 때문에 형식이 동일하다.
- SQLite: 파일형 데이터베이스. 간단하게 DB 구축하고 실험하기에 적합하다. 하지만 기능이 제한되기 때문에 고급 쿼리는 실행하기 어렵고, 서버를 띄우고 작업하지 않기 때문에 실행중인 메모리 상에 상주하게 된다면 데이터 손실도 생길 수 있다.
	- 연결: Connection을 establish하고 소통할 수 있는 cursor 만들어서 통신.
		```python
		import sqlite3
		
		# 연결 세션 보관
		con = sqlite3.connect('dbfile.db') # dbfile.db 파일 불러오기. 확장자 .sqlite3 등이 있다. 없으면 생성
		# con = sqlite3.connect(':memory:') # 메모리에서 실행
		
		# 소통 cursor
		cur = con.cursor()

		# Query 실행
		cur.execute('''
			CREATE TABLE my_table (
				id INTEGER PRIMARY KEY,
				cell VARCHAR(32)
			);
		''')

		id_0, cell_0 = 2, 'something'
		id_1, cell_1 = 4, 'nothing'
		records = [
			(id_0, cell_0),
			(id_1, cell_1)
		]

		cur.execute('INSERT INTO my_table VALUES (?, ?)', (id_0, cell_0))
		# for record in records: cur.execute('INSERT INTO my_table (id, cell) VALUES (?, ?);', record)

		# commit
		con.commit()

		# 결과 조회
		cur.fetchall() # fetchmany fetchall # return result one by one
		```
- 클라우드/원격 데이터베이스. 반대는 온프레미스
	- URI uniform resource identifier: 리소스 / 정보 찾고 받아올 때 사용
		- 형식: `서비스://유저_이름:유저_비밀번호@호스트:포트번호/경로`
		- 프로그램에 포트번호가 있을텐데, PostreSQL은 `5432`
	- 연결: `psycopg2`로 파이썬과 PostgreSQL 데이터베이스와 연결 가능
		```python
		import psycopg2

		conn = psycopg2.connect(
			host="서버 호스트 주소",
			database="데이터베이스 이름",
			user="유저 이름",
			password="유저 비밀번호")
		```
- 배열: 연속된 공간. 하나의 값을 지워준다면 그 지운 것을 매꾸어주어야 한다. 즉 앞으로 한 칸씩 당겨짐. 삭제 삽입이 비효율적
- 연결리스트: 0, 1, 2가 있을 때 1을 지워도 0과 2로 연결이 된다. 하지만 비연속적인 공간이라 검색이 느릴 수 있다.
	- 연결리스트의 시작 head 끝은 tail 후 NULL.
	파이썬은 자동으로 메모리 정리해줌. garbae collection. 그런데 진행동안 프로그램 스톱되어 느려질 수 있다.
	또한 언어 레벨에서 포인터도 지정해주어서 ㅇㅋ

시간복잡도: 얼마나 시간 걸리는지 계산해볼 수 있다.
	- for 안에 for가 들어 있으면 O(n^2)
	- for 하나는 O(n)

sorting: easy sorting, standard sorting
	- easy sorting
		- 삽입: 지금 하나와 다음 하나를 비교해볼 수 있다.
		- 선택: 전체 중 하나 작은 값을 맨 앞으로 보냄
		- 버블: 버블: 삽입과 비슷함.
		- 시프트가 많다.
		- 전부 다 O(n^2) 너무 길다.
	- standard sorting
		- 셸 정렬: 건너 뛰어서 비교하는데, 간격을 점차 줄인다.
		- 힙 정렬: 힙 트리 - 이미 정렬되어 정리되는 구조에서 하나씩 꺼냄.
		- 퀵 정렬: 임의의 pivot을 기준으로 왼쪽끼리 정렬, 오른쪽끼리 정렬. 힙과 비교했을 때 구조를 저장할 메모리가 필요 없어서 퀵을 사용한다.
		- 병합 정렬: 균등한 크기로 나누어 정렬. 퀵의 임의와 다름. 합병할 때 추가적인 공간이 필요하기 때문에 우선순위에서 밀려남.
		- 기수 정렬: 비교 연산 없이 정렬하는 알고리즘. 자릿수에 대한 큐에 삽입하고 꺼내는 방식으로 한다. 단점으로는 자릿수마다 보관할 공간이 필요함.

stable sorting vs. unstable sorting: 중복 요소의 순서가 바뀔지 안 바뀔지에 대한 부분

binary search, deque, hashmap 찾아보기.