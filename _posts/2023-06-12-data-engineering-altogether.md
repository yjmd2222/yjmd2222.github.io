---
title:  "AIB 데이터 엔지니어링 실전 예제 생각해보기"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, time, encoding, api, selenium, scraping, db, scheduler]
date: 2023-06-12 09:00
last_modified_at: 2023-06-12T09:00:00-09:00
---

1. 데이터를 웹스크레이핑(Selenium)이나 API로 액세스 / 다운로드
  - Data acquisition timestamp 같이 표기
2. 데이터베이스에 얻은 데이터 적재: NoSQL
3. 데이터 정제 방법 고안 후 RDB에 적재
4. 전체 과정 스케줄러로 짜기

이후 할 수 있는 것들: 대시보드에 업로드 / 연결