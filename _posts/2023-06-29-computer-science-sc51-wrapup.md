---
title:  "AIB 섹션 5 컴퓨터공학 스프린트 1 랩업"
excerpt:
header:
  # teaser:
categories: [AIB_Section_5]
tags: [AIB, computer_science, problem_solving]
date: 2023-06-29 09:00
last_modified_at: 2023-06-29T09:00:00-09:00
---

예외처리 예시: 로그인할 때 잘못 된 아이디 비밀번호가 입력되었을 때 별도로 처리해줘야 한다. 이 부분이 예외처리. 좋은 웹사이트는 각 상황마다 드롭다운이 바뀐다. 좋지 않은 웹사이트는 일관적이지 않음.

![](/assets/images/s5cc00-0-cv.png)

![](/assets/images/s5cc00-1-cv.png)

### 내가 무엇을 배웠나 생각해보기
반복문과 조건문을 통해 비정형화된 코드를 작성할 수 있는가. 일단 어떤 문제가 주어지면 반목 조건 함수 클래스 작성은 할 수 있는데, 끝까지 풀 수 있는지는 모르겠다. 답이 안 나오면 챗한테 물어보거나 구글 검색중인데, 편리한 기능들이 너무 많아서 이걸 스스로 구현해보려는 노력을 잘 안 함.

#### 511
파이썬에서의 문제해결은 당연히 코드 짜는 것인데, 이를 반복문과 조건문으로 작성하면 모든 것을 구성할 수 있다. 여기에 내장 메소드나 별도 찾아볼 수 있는 라이브러리로부터 가져온 것들을 더한다. 글로써 문제를 스스로 설명해보고 이를 코드로 옮겨오는 과정을 거친다. 여기서 자료구조를 잘 선택해야 한다. 특정 상황에 특화된 자료구조들이 있기 때문에 이를 사용하면 아주 손쉽게 문제를 풀 수 있지만, 전혀 맞지 않은 자료구조로 문제를 풀라고 하면 난감하다. 키워드: 자료구조, 조건문, 반복문.

find와 index의 차이점: find는 보통 어떤 자료 구조 내에 해당하는 값이 있을 때 사용하고, index는 그 해당 자료가 어디에 위치에 있는지를 알려주는 포인터라고 생각할 수 있을 것 같다. 비슷한 것으로는 dictionary `.get`이나 사각괄호 사용하는 `__getitem__`이 있을 것이다. list에서는 `.index()` 있는데, `for idx, val in enumerate(iterable)`를 사용해서 `list_[idx]`를 더 많이 사용해본 것 같다. `list_.index(item)`을 하게 되면 처음부터 `item`이 있는지 확인해야 하기 때문에 시간 오래 걸리는데, 애초에 index를 다뤄야 하는 상황이 물론 상황마다 다를 수는 있지만 어떤 값을 다루고 그 값의 index를 별도로 보관하는 것을 많이 접해봐서 그럼.

for, while문을 서로 바꿔서 작성할 수 있다. for은 iterable을 하나씩 옮겨가면서 반복하는 것, while은 별도 조건/반복상황을 작성해서 그 조건이 더이상 만족하지 않을 때까지 반복. while이 어떻게 보면 훨씬 더 자유롭다. 조건을 `idx += 1`이 아니라 `idx *= 2`로 하면 for loop이 하나씩 옮기는 것과는 다르게 두 배씩 옮겨갈 수 있다.

`[1,2,3,4] < [1,2,4] == True`라고 한다. 일단 내 답을 먼저 들어봤지만, 왜 그런지 처음부터 다시 생각해보면 인덱스끼리 매칭시켜서 비교하면 두 번째에서 `3 < 4`라서 그렇다고 생각할 수 있을 것 같다.

#### 512
나는 의사코드를 작성해본 적이 거의 없었는데, 진짜 중요하다. 내 뇌는 컴퓨터 프로그래밍 식으로 돌아가지 않고 언어로 생각을 표현한다. 그러면 프로그램 짜기 전에 물론 간단한 것이라면 곧바로 할 수 있겠지만 코딩테스트를 한다고 하면 진짜 헷갈리는 것 많이 나올 것이다. 그래서 곧바로 코드 작성하기 전에 문제를 언어로 설명하고 이 문제를 어떻게 풀어야 할지 의사코드를 먼저 작성해본다면 내가 어느 단계에서 어떤 코드를 작성해야 하는지를 상황파악에 많은 도움이 된다. 특히 DP나 재귀 문제는 의사코드 작성 안 하면 머리 터질것 같다. 물론 많이 반복해본다면 쉽게 할 수 있겠다 라는 희망을 가져본다. 예외처리는 매우 중요하다. 내가 긴 코드를 짰을 때 예외적인 상황이 발생할 수 있을 것이라는 생각을 해보기. 현재 사용한 한 줄에서 어떤 예외가 발생할 수 있는지 라이브러리나 내장메소드 설명 읽어보면 무슨 예외처리 해야 하는지 감이 좀 온다. 예를 들어 dictionary에 Key가 없으면 KeyError가 날 테니 차라리 `dict_.get(key)`를 하고 그것이 True인지 None인지 확인하는 방법이 있겠다. 그리고 한 줄에서 다양한 에러가 날 수 있을텐데, 각각 `except SomeError as error:`로 구분해서 각각의 상황에 대응하는 코드를 작성해볼 수 있겠다. 그리고 의도적으로 맘에 들지 않는 상황에서 `raise SomeError('error message')`를 해보기. 실제로 이런 형식으로 에러가 나서 `except`로 잡아오는 것이 되겠다.

로봇청소기로 의사코드를 작성해보기
```python
if 배터리 없으면:
  작동 안 함
바닥 = 각 위치와 그 위치로부터 동서남북 방향이 저장된 자료구조
현재위치 = 바닥 중 처음 index
while True:
  for 방향 in 현재위치:
    if not 방향: # 이 방향에 청소 안 했으면
      move(방향)
      현재위치[방향] = True
  if all(현재위치.keys()):
    현재위치 += 1
  if all(all(val) for val in 바닥.values()):
    break
print('작업 완료')
```

엘리베이터 의사코드 작성해보기
```python
# 버튼을 누르면 오고, 먼저 어디 가고 있으면 그 위치를 곧바로 바꾸진 않을 것.
# 가는 방향에 내가 있으면 나를 먼저 태울 것이고 그렇지 않으면 난 기다려야 함
# 가지 않는 방향이면 마찬가지로 방향 전환할 때까지 기다리기
# 방향 전환은 현재 진행중인 방향에서 기다리는 사람을 태우면 그 때 전환할 것

while True:
  if 밖에서 버튼 누르면:
    방문할 위치.append(누른 위치)
  if 안에서 버튼 눌러도:
    마찬가지로 append
  방문할 위치.sort(key=현재 방향과 일치하게)
  현재 위치 = 엘리베이터.이동(방문할 위치[0], 한층씩)
  if 방문할 위치 and 현재 위치 == 방문할 위치[0]:
    엘리베이터.도착()
    방문할 위치.pop(0)
```

컴프리헨션과 기존 반복문은 상황에 따라서 속도가 각각 다르다. 예를 들어서 별도 조건 없이 컴프리헨션으로 리스트를 만드는 것과 반복문 속에서 list에 append하는 것을 비교하면 하나씩 더하는 것이 더 오래 걸린다. 그런데 컴프리헨션에서 조건을 넣는다거나 list를 만드는 것이 아니라면 반대가 더 빠를 것이다. 예를 들면 print하는 것을 list comprehension을 사용할 필요는 없다.

#### 513
oop는 객체를 저장하고 반복해서 사용한다는 개념으로 이해한다. 객체라고 하면 파이썬에서는 어떤 것이든 다 객체가 될 수 있는데, 예를 들면 함수도 객체, 클래스 인스턴스도 객체, 그리고 자료 구조도 다 이를 기반으로 만들어서 객체이다. Oop는 일종의 프로그래밍 패러다임이다. 예를 들어 객체를 사용하지 않는다면 코드를 저장하지 않는다는 것으로 볼 수 있고, 똑같은 동작을 나중에 다시 사용하고 싶을 때 다시 코드를 작성하는 번거로움이 생긴다. OOP에서 가능한 모든 것을 객체화 시키면 객체들이 서로를 사용하고 서로의 속성도 활용하는 상호작용을 만들 수 있다. 여러 가지 세부개념이 있어서 다시 한 번 상기하기 위해 나열하면 상속, 포함, 추상, 다형성, 캡슐화가 있다. 상속은 부모/base가 되는 클래스로부터 자녀 클래스가 attribute나 method를 다시 작성할 필요 없이 그대로 받아올 수 있는 것. 이를 새로 작성할 코드의 일부로 포함시키면서 내용을 바꿀 수 있는데, 이것을 다형성이라고 한다. 포함은 A 클래스의 attribute로 다른 객체 B를 포함시켜서 B의 속성이나 메소드 등을 이용할 수 있다. OOP에서 여러 클래스가 있을 때 이를 어떻게 구상하면 될지에 대한 개념이라고 생각한다. 설계도만 먼저 만들어놓고 기능은 전혀 구현하지 않았지만, 나중에 자녀 클래스에서 점차 채워나갈 것으로 예상하고 틀을 먼저 짜 놓기. 캡슐화는 특정 기능들을 묶는 개념이다. 클래스 안에서 이 클래스 instance로 무엇을 할 수 있는지를 attribute/method로 포함시켜 놓는다. 그래서 이렇게 캡슐화를 해놓으면 접근 제어도 생각해볼 수 있겠다. 그리고 말로 표현하면서 헷갈리는걸 지금 좀 정리해보려고 하는데, 클래스는 객체의 설계도, 즉 객체가 어떻게 돌아갈지에 대한 것들이 다 정의되어 있다. instance는 이 객체에 메모리를 부여해서 runtime에 실제로 작동하는 것으로 구분할 수 있을 것 같다.

클래스 `__init__`을 사용하지 않고 속성 부여해보기. 이건 두 가지로 생각해볼 수 있을 것 같다. 클래스 자체에 해당하는 속성이거나, 별도 메소드로 초기화가 아닌 메소드 호출 시에 속성을 만드는 기능.
```python
class MyClass:
  attr_1 = 1

  @staticmethod
  def method_1():
    return 1

print(MyClass.attr_1, MyClass().attr_1, MyClass.method_1())

class MyOtherClass:
  def init(attr_1):
    return attr_1

print(MyOtherClass().init(1))
```

#### 514
Big O. 시간이 얼마나 걸릴까, 공간을 얼마나 잡아먹을까. 각각 설명해보자면 어떤 operation이 일어날 때 얼마나 시간이 걸리는지가 시간복잡도이다. 자료구조에 따라서 비슷한 기능의 시간이 서로 다를 수 있다. 예를 들어 dictionary는 키를 index와 연결/변환해서 사용하고 있어서 조회할 때 O(1) 상수시간 즉 각각의 값을 읽어가면서 이 값이 맞는지 확인할 필요 없이 index로 곧바로 조회되는 시간복잡도를 가진다. 반면 리스트는 인덱스 자체를 넣어서 조회하면 그 값이 나오긴 하지만 그게 아니라 그 값이 있는지를 확인하려면 하나씩 그 값이 맞는지 넘어가면서 확인해야 해서 선형시간이 걸린다 O(n). 그래서 주어진 iterable에 대해 한 번 다 돌면 선형시간, 한 번 돌리는 개념이 아니라 곧바로 나오면 상수, 선형보다 짧은 것, iterable을 두 번 돌리는 경우 O(n^2) 등이 있다. 선형보다 짧으면 보통 O(log n)이라고 생각하면 대부분 맞는데, 가장 쉬운 예시로는 while loop을 돌릴 때 그 조건을 i *= 2로 하면 2씩 커지니까 log base를 2로 잡고 iterable의 사이즈를 log에 넣어 나온 결과가 루프 돌아간 횟수이다. binary search가 이것을 잘 활용한 예시. 정렬된 리스트에서 값이 있는지 확인해보려면 그냥 for loop 돌리면 선형시간이다. 그런데 binary search는 이미 정렬되었다는 사실을 이용하는데, 절반으로 나누어 절반의 값이 찾고자 하는 값보다 큰지 작은지를 따져서 크면 오른쪽 절반을 선택, 작으면 왼쪽 절반을 선택해서 거기서 반복한다. 그러면 돌릴 루프 자체가 절반으로 줄어들기 때문에 log base 2로 log n 시간이 걸린다. 공간복잡도는 개념적으로만 매우 얕게 다뤘는데, 할당 가능한 메모리 중 얼마만큼 차지하는지이다. 가장 쉬운 예로 byte 하나가 들어가는 리스트는 선형, nested list는 제곱이라고 생각할 수 있다. 나중에 그래프 배울 때 인접리스트 vs. 인접행렬이 있는데, 인접행렬은 nested list이기 때문에 n 제곱, 인접리스트는 n개의 노드들과 이를 서로 잇는 간선이 m개라서 n+m의 선형이다.

크기가 N인 리스트에서 모든 요소들을 제거하기 위해 remove, pop, delete를 사용해볼 수 있을 것이다. `remove`는 index를 조회해야 하기 때문에 O(n), `pop()`은 맨 끝 제거하는 것이라 끝을 알고 있어서 O(1), `del`도 index를 알고 제거하는 거라서 O(1). `pop(0)`을 하면 맨 처음 것을 제거하는 거고, `pop(n)`은 n번재를 제거하는 건데, 파이썬 리스트는 인덱싱하고 있어서 중간에 하나 빈 것을 끝 것들을 하나씩 다 옮기느라 O(n) 시간 걸린다.