---
title:  "AIB 섹션 6 - 팀 프로젝트 1 - 1일차"
excerpt:
header:
  # teaser:
categories: [AIB_Section_6]
tags: [AIB, team-project, team-project-1]
date: 2023-07-19 09:00
last_modified_at: 2023-07-19T09:00:00-09:00
---

의사소통과 문서화 실력을 길러보자

구현하고 싶은 것: 국내 여행 비용
### 서비스 기능 1
- 목적지까지 이동수단
- 목적지 숙박비용
- 목적지에서의 이동수단
  - 렌트카
  - 택시투어
  - 기타 대중교통
### 서비스 기능 2
- 항공사, 차 브랜드 선택 기능
- 날짜 (요일/주말/주차/성수기)
### 서비스 기능 3 (추가 기능)
- 날씨
- MBTI I/E에 따른 활동 추천
- 최저가

### 개인 구상
- 홈페이지에 접속하면 기본적으로 목적지까지의 이동수단을 숙박 종류, 목적지에서의 이동수단을 선택할 수 있어야 한다. 해당 사항이 없으면 취합했을 때 최종비용에서 제외.
  - 비용 관련 모델/서치 세 가지(네 가지: 활동 추천할 시)로 구분
  - 각각의 모델의 비용을 합쳐서 최종금액 표기
  - 패키지가 있다면 별도(예외상황) 처리
- 기본요소를 선택하면 서비스 기능 2에서 세부적으로 선택할 수 있을 것.
  - 그런데 날짜는 제일 먼저 선택할 요소임. 날짜, 브랜드가 input 특성이 됨.
- 날씨는 날짜 선택하면 목적지 날씨 표기
- 활동
  - 성격 상관 없이 모든 활동 나열. search api를 통해 `f'{해당 장소명} 투어'` 등으로 검색.
  - 성격을 선택한다면 검색어에 성격에 알맞는 키워드 추가
- DB sorting vs. 머신러닝
  - sorting은 여행날짜 데이터를 표기할 수 있어야 함
    - 실시간으로 api로 끌어오기: 비용 발생, 속도 저하
    - 주기적으로 db 최신화해서 최신화 날짜 표기
  - 머신러닝은 예를 들어 1,2년 데이터를 학습시켜서 금액 표기.
    - 주로 저가 비용에 해당하는 것으로 학습시키고, 예상 노드와 비용을 표기. 이 비용 기준으로 표준편차로 해서 주위에 해당하는 값들을 표기...

### 합의한 구상안
- 머신러닝 사용 안 함
- 금액으로 등급을 나눔
- 금액 내 높은 별점 최상위 여러 개 경로 표기
  - 별점은 추후 부여.