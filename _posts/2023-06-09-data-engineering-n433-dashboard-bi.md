---
title:  "AIB 데이터 엔지니어링 433 Dashboard and Business Inteligence"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, business_inteligence, bi, metabase, looker_studio]
date: 2023-06-09 09:00
last_modified_at: 2023-06-09T09:00:00-09:00
---

어떤 것의 보고서 만들 때 네 가지로 정리해볼 수 있다.
1. 워드, 한글 같은 보고서
2. 코랩 (DS)
3. 웹 애플리케이션. 개발비용 지출
4. BI를 이용해서 데이터베이스와 연결해서 데이터 변경될 때 보고서 내용도 같이 업데이트. DA가 많이 쓰는데, DE, DS도 활용 가능해야함.
  - 결과 보고 및 데이터 파이프라인 확인 가능.

*데이터 민주화*

Metabase, Looker Studio. Power BI, tableau 등. tableau가 점유율 가장 높음.

BI: 기업 비즈니스 의사 결정 지원

과정
- 데이터 수집 및 가공 (SQL)
- 비즈니스 상태 확인
- 데이터 시각화로 결과/전략 제시
- 그 결과 바탕으로 대응

돌아가서 SQL 다시 공부 잘 해봐야할듯.

엑셀에 비해 BI 툴이 기능이 훨씬 좋음. 실시간 업데이트, 리포팅 기능, 플래닝 기능, 양방향 vs. 단반향.

실시간으로 확인할 수 있는 보고서. 보기 쉬운 보고서.

key performance indicator KPI: 비즈니스 입장에서는 판매량, 매출증감, 고객유입량, 클릭횟수 등이 중요한 지표일듯.

신기했던 점. 아직 배우는 단계라 잘은 모르겠지만, 3000번 포트를 사용중인 Metabase를 종료한 후 컨테이너를 다시 실행하려 보니 명령어를 몰라서 새로 생성하는 커맨드를 입력했다. 그런데 실행 안 된다는 오류 뜨고 bash나 terminal에 실행 중이 아닌데 컨테이너가 활성화 되었다. docker가 어떻게 되는지 잘 모르겠음...