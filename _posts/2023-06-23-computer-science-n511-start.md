---
title:  "AIB 섹션 5 컴퓨터공학 N511 시작"
excerpt:
header:
  # teaser:
categories: [AIB_Section_5]
tags: [AIB, computer_science]
date: 2023-06-23 09:00
last_modified_at: 2023-06-23T09:00:00-09:00
---

- 데이터 과학자에게 필요한 우선순위란? 의사소통능력
- 데이터 직군으로 어떤 것을 계속 학습해야 하는지
- 내가 생각하는 프로그래밍은?
- 지금까지 학습/실습한 내용 중 프로그래밍이 얼마나 중요한가
- 생각하고 프로그래밍해보기. 고도화, 추상화, 효율성. 내가 결과를 도출하는 그 과정 자체에 대해 고민해보기

키워드: 반복문과 조건문, 내장 메소드, 프로그래밍

배울 것: 파이썬의 다양한 활용법, 메소드의 활용법, 컬렉션 자료형의 활용

프로그래밍에서의 문제 해결은 단순히 결과만 내는 것이 아니라 과정을 생각해보아야 한다. 그런 의미에서 세 가지
- 복잡하고 규모가 큰 문제를 작은 단위로 분할하면서 해결
- 문제 내에서 특정 패턴을 찾기
- 문제를 최소한의 비용으로 최대한 빠르게 해결하기

위에서 첫 둘은 그 자체로 설명이 잘 되는데, 세 번째는 비용이라는 용어를 생각해보아야 한다. 시간이 될 수도 있고, 파워나 유지 비용이 될 수도 있다. 이를 주어진 상황에 알맞게 고려하여 코드 짜기.

그리고 미리 알고 있기로, 각 자료형마다 연산의 속도가 다르다. 이를 이제부터 자세히 알아볼 것 같다.

## 정규표현식
- 복습할 것
  - search: str에서 패턴 찾으면 match object 반환
  - search: str 처음부터 패턴 찾으면 match object 반환
  - compile: search하기 위한 object 생성
  - span: match된 것의 index start to end
  - group: subgroups. 전체 매치, subgroup 1, subrgoup 2,...
  - escape characters
  - raw string
  - copy: copy하는 변수는 copy하지만 그 내부 변수는 원본과 연결. `import copy`의 `copy.deepcopy()`하면 내부까지 다 새로 copy
  - extend: 주어진 자료형을 이어 붙이기. append: 주어진 자료형을 element로 추가하기. insert: 해당 위치에 주어진 자료형 element로 추가하기.
  - functools reduce: `reduce(lambda acc, what_to_add: acc + what_to_add[subscript], from_what, 0)` 여기서 0는 몇부터 acc 시작할지. 0부터 다 더한다는 식으로 생각해볼 수 있다. 명시하지 않으면 첫 `what_to_add` 부터 더함.
  - swapcase: upper는 lower, lower는 upper로.

여러 기능들이 메소드로 구현되어 있을 것이다. 내가 하려고 하는 것이 무엇인지를 메소드로 가져와보기.

용어 정리: 모듈, 패키지, 라이브러리, 프레임워크
- 모듈 안에 object: 변수, 함수, 클래스가 존재할 수 있다
- 패키지: 모듈의 모음
- 라이브러리: 패키지의 모음
- 프레임워크: 라이브러리의 모음.