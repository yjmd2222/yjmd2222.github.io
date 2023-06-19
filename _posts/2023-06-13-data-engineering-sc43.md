---
title:  "AIB 데이터 엔지니어링 섹션 4 스프린트 3 랩업"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering]
date: 2023-06-13 09:00
last_modified_at: 2023-06-13T09:00:00-09:00
---

이번 스프린트/섹션 데이터 전문가의 공통역량에 대해 배움

API 대시보드 완성 목표

docker에서 주의 깊게 기억해야 할 내용
- 이미지, 컨테이너, 레지스트리 용어 구분
- 이번 주 배웠던 내용 연습 좀 많이 해보기. 이게 거의 기본.

jinja: flask와 html 연결

CRUD

분석가에게는 대시보드 매우 중요함

![Alt text](/assets/images/s4de30-3-dashboard.png)

성장 가능성 중요. 아는 것도 중요하지만 앞으로 내가 스스로 무엇을 할 수 있는지.

DE는 파이프라인; 분석가는 문제정의, 지식전달;

etl: extract transform load

de 포트폴리오는 파이프라인

개발 트렌드: https://insights.stackoverflow.com/survey/2022 https://survey.stackoverflow.co/2022/ https://insights.stackoverflow.com/survey

- 지난 프로젝트를 웹에 연결시켜보기
- 카메라 액세스 있어야함
- 이미지 저장이 필요하다면 로컬에 저장. 그렇지 않다면 인메모리(웹 상)
- 이미지를 웹 상의 MoveNet으로 읽기
- 읽은 데이터 db에 적재 / DNN에 입력
- 결과 반환

필수조건: 데이터 수집, 적제, 머신러닝 모델 웹에 탑재

API 서비스 개발: 웹페이지에 사용자가 데이터를 입력하면 데이터를 기반으로 머신러닝 결과를 반환해주는 것

adv: 모델 업데이트 하기 위해 db에 데이터 저장

- https://wikidocs.net/81048
- http://www.tcpschool.com/html/intro

##

모델이 필요하고, 데이터도 필요함. 상단에 추천.

고객정보와 상품정보를 바탕으로 추천하는데, 그 정보는 데이터베이스에 있음.

정리하자면
1. 옷 추천 모델
2. 추천할 상품
3. 추천될 장소(서비스) -> 웹

데이터 수집, 전처리, 모델 학습.

데이터베이스에 있는 상품을 추천. 데이터베이스에 추천된 상품을 구매하는지 아닌지에 대한 정보 적재. 즉 테이블이 여러 개가 된다.

웹 상에 배포.

데이터베이스가 가장 중요함.

도전과제 배포: 외부에서 접근.

파이프라인 예제: 수집, 적재, 모델, 플라스크, 대시보드