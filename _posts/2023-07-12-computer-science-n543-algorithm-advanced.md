---
title:  "AIB 섹션 5 컴퓨터공학 N534 Algorithm Advanced"
excerpt:
header:
  # teaser:
categories: [AIB_Section_5]
tags: [AIB, computer_science, algorithms]
date: 2023-07-12 09:00
last_modified_at: 2023-07-12T09:00:00-09:00
---

- 실생활 속 알고리즘은 무엇이 있을지
- dynamic programming(문제 해결을 위한 접근 방법 중 하나. 불필요한 계산 줄이기)에 대해 무엇을 이야기할 수 있을지. 즉흥적으로 문제에 접근해보기.

- 숲을 보는 시점으로 생각하기. 큰 그림을 보고 세부적으로 어떻게 작동할지로 구분.

키워드: dynamic programming, greedy, algorithm

### Dynamic Programming: 동적 계획법
문제의 일부분을 풀고 그 결과를 재활용해서 전체 문제를 해결하는 방법. 분할정복의 개념 + 기존 결과 활용.

코드를 분석할 때
- 그림 그려보기
- 말 바꿔서 표현해보기
- 코드 흐름을 이해해보기

분할정복과의 차이점: DP가 상위 개념.
- DP는 중복되는 서브문제에 대해 memoization을 적용
- DC는 기억 안 함. 각각의 문제가 독립적

memoization(하향): 큰 문제에서부터 분할할 때 중복되는 부분을 다시 계산하지 않기 위해 기억.
tabulation(상향): 가장 작은 문제부터 해결해서 메인 문제 해결. 작은 부분에서 큰 부분으로 올라가면서 작은 부분 기억할 필요 없을 수도 있다.

의사코드
```python
# memoization
def f(n):
    if n in memo:
        return memo[n]
    else:
        # 계산 과정
        result = ...
        # 결과를 메모리에 저장
        memo[n] = result
        return result
```
```python
dp = [0]*(n+1)
# 점화식에 따라 DP 테이블을 갱신
for i in range(1, n+1):
    # 계산 과정
    dp[i] = ...
```

물론 둘 다 DP이기 때문에 분할정복의 개념이 들어가 있다.

정리해보면 __DP는 최적 부분 구조로 구성된 중복 서브문제를 분할정복으로 해결한다__

다양한 입력조건에 따라 다양한 문제해결방법이 있을 수 있음. 최적의 알고리즘을 생각해보기

대표적인 DP 문제: [배낭](https://howudong.tistory.com/106), fib수열

## Greedy: 탐욕
중복을 고려하지 않음

최적과는 다르게 한 단계씩 빠른 시간 내에 해결하기. 서브문제들 독립적으로 간주.

각 단계를 빠르게 해결하는 것만 생각하기 때문에 전체 과정의 비용을 생각하지 않는다.

근사 알고리즘의 성능: 얼마나 빠른지, 최적해에 얼마나 가까운지.

탐욕은 정답과 시간 간의 트레이드오프가 있음.