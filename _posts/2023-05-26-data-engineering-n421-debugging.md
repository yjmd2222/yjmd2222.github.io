---
title:  "AIB 데이터 엔지니어링 421 디버깅"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, 디버깅]
date: 2023-05-26 11:00
last_modified_at: 2023-05-26T11:00:00-09:00
---

- 키워드: 디버깅, pdb, pythonic, 함수, 클래스
- [ ] design patterns

파이썬 철학: 그냥 시작해보기. 리뷰를 통해 pythonic한 코드로 만들기 - 간결하게.

함수: 기능. 어떤 기능을 수행하는 무언가.
  - 메소드: 객체와 관련된 것. `object.method()`

클래스: 통일 된 양식으로 object들을 정리할 수 있다.
- `__init__()`: 클래스 선언하는 순간 / 클래스 object 생성시 실행
- `self`
  - method 안에 넣음. method 형태로 사용하겠다.
  - attribute 만들 때 `self.attribute1` 형식으로 한다.
- 상속: 원본의 내용에 추가하는 새로운 클래스 만들 수 있다.
  - `class Child(Parent):`로 선언
  - `__init__()` 아래 `super().__init__()`로 `__init__()` 상속받을 수 있음

### 디버깅
코드 에러 발생 시 잠깐 멈추고 상황 살펴보기. 디버그: 버그를 찾는 행동

```python
# python 3.7 이전
import pdb

action()
pdb.set_trace()
```
```python
# python 3.8 이후
action()
breakpoint()
```

`(Pdb)`로 시작하는 line이 terminal에 뜨고 거기에 변수 이름을 입력해서 값을 보고 바꾸거나 c, l, s, n을 입력할 수 있다.
- c: continue. 다음 debugger instance로 이동
- l: list. 다음 11줄 나열
- s: step. 다음 문장으로 이동하고, 함수는 내부 탐색
- n: next. 다음 문장으로 이동하고, 함수는 return 값을 받음

위와 중복되지 않도록 한 글자 변수명 만들지 않기.

추가로 다양한 작업을 할 수 있는데, [documentation](https://docs.python.org/3/library/pdb.html) 찾아보기.

### 함수
- 특정 기능을 반복적으로 실행할 수 있는 코드. 매 번 새로 입력하지 않아도 된다.
- 함수에 파라미터를 정의한다면 파라미터에 argument/인수를 넣어 이에 대해 함수 내에서 추가적인 작업을 한다.
- 필드 인수: 입력하지 않으면 실행 안 됨. 키워드 인수: 어떤 파라미터에 대해 입력하는지 명시하여 순서 지키지 않아도 된다. 기본 인수: default로 사용할 인수값
- return: 이 값을 반환하고 종료한다. 비어있으면 그저 종료

### 클래스
- 객체지향언어 기반으로 설계된 파이썬. 설계도를 만들고, 그 설계도로 결과물을 만드는게 합당하다. 즉 대규모 프로젝트에서 코드 재사용을 줄이기 위해 함수와 클래스를 사용한다.
- self. 클래스 인스턴스(self, 자기 자신)에 대해 method를 실행하겠다는 의미. 이것이 필수 인수가 된다. 다만 메소드를 실행할 때에는 자동으로 넘어가서 실행되고, 이것이 메소드 정의에 나열되어 있지 않다면 불일치 오류가 나타난다.
- 클래스 메소드 중 특별한 것들이 있는데, 대표적으로 생성자 `__init__()`. 클래스 인스턴스 만들 때 사용되는 메소드이다.
  - instance가 가지고 있는 변수를 미리 지정해줄 수 있다.
  - instance가 가지고 있는 변수와 클래스 자체가 가지고 있는 변수는 다르다.
- `@property`: A가 B에 종속되어 있을 때 B의 값을 변경해준다고 생각해보면 B는 A가 정의된 이후에 변경해준 것이라 A는 바뀌지 않는다. 이를 바꿔주기 위해 `@property` 이용한다.
  - 변수를 메소드처럼 입력해서 return해준다.
    ```python
    @property
    def att_B(self):
      return func_1(self.att_A)
    ```
- setter: 변수에 직접 접근하지는 않지만 추가 기능을 포함시키고 싶을 때 사용한다. @property 이후 사용
    ```python
    @att_B.setter
    def att_B(self, something):
      self.att_A = self.att_B + something
    ```
    instance `my_object`에 `my_object = '!!!'`를 한다면 `!!!`가 `something`으로서 뒤에 추가된다.
- _, __ dunderscore: 둘 다 숨기고 싶은 내용, 수정하지 않았으면 하는 내용이다. _는 그 값이 일반 object와 같이 행동하고, __는 class name을 통해서 확인할 수 있다: `my_ins._MyClass__dunderscore`. 즉 완전히 숨겨지지는 않는다.

### 데코레이터
모든 함수는 객체이기 때문에 함수 안에 함수를 넣을 수 있다. 이를 활용한 부분이 데코레이터. 위 내용들이 decorator이다.

wrapper function을 정의하고 그 function을 `@wrapper`로 내부 함수 윗줄에 입력하면 적용된다. 그런데 함수마다 인자가 다를 수 있고, wrapper를 그 함수마다 다르게 정의하기는 힘들다. 그래서 args and kwargs를 이용한다.
```python
def wrapper(func):
  def to_do(*args, **kwargs):
    do_something_1()
    func(*args, **kwargs)
    do_something_2()
  
  return to_do

@wrapper
def my_func(something):
  print(something)
```

### 디스크립터
객체의 속성을 조회, 저장, 삭제를 제어할 때 사용되는 객체이다. 각각에 대해 `get()`, `set()`, `delete()`이다. 조회 저장 삭제를 할 때 각각이 호출된다.

용도로는 여러 가지가 있는데, attribute 값을 검증, 변환, 기록에 사용되고, 액세스를 권한 관련해서 사용할 수 있다.

```python
# ex 1
class OneDigitNumericValue():
    def __init__(self):
        self.value = 0
    
    def __get__(self, obj, type=None) -> object:
        return self.value
    
    def __set__(self, obj, value) -> None:
        if value > 9 or value < 0 or int(value) != value:
            raise AttributeError("The value is invalid")
        self.value = value

class Foo():
    number = OneDigitNumericValue()

breakpoint()
```

`__delete__`가 없어서 삭제하려 하면 없다고 에러 난다.

```python
# ex 2
class PrintOperation:
    def __init__(self):
        self.value = 0
    
    def __get__(self, obj, type=None):
        print('getting the value', self.value)
        return self.value
    
    def __set__(self, obj ,value):
        print('setting the value as', value)
        self.value = value

    def __delete__(self, obj):
        print('deleting')
        del self.value

class Bar:
    att = PrintOperation()

breakpoint()
```

### Generator
제너레이터는 large data를 다룰 때 용이하다. 한 번에 하나의 변수 안에 큰 데이터를 로드한다면 메모리 초과하는 상황이 발생할 수 있는데, 제너레이터는 값을 한 번에 하나씩 생성하기 때문에 메모리를 절약할 수 있다.

```python
def my_generator():
  for i in range(10):
    yield i

for i in my_generator(): print(i)
```

### Abstraction
The process of hiding unnecessary information from the user. Hides internal functionality of the object/function/class from the user. The user can use the function, but the way it works is hidden from the user. This is to reduce complexity and improve efficiency of the application.

ABC: Abstract Class. abstract method를 사용하는 클래스. Base class로부터 상속받고, 공유하는 concrete method가 있거나, 각각의 child class별로 다른 method들이 있을 것이다.

```python
from abc import ABC, abstractmethod

class Parent(ABC):
  #common function
  def common_fn(self):
    print('In the common method of Parent')
 @abstractmethod
  def abs_fn(self): #is supposed to have different implementation in child classes 
    pass

class Child1(Parent):
  def abs_fn(self):
    print('In the abstract method of Child1')

class Child2(Parent):
  def abs_fn(self):
    print('In the abstract method of Child2')
```

ABC로부터 inherit하면 다음과 같은 규칙 적용:
- Abstract Class에 있는 abstract method를 다시 정의하지 않으면 에러 발생

나만의 example 만들어보기

```python
from abc import ABC, abstractmethod
import numpy as np

class Shape(ABC):
  def dimension(self):
    print('a 2D shape')

  @abstractmethod
  def perimeter(self):
    pass

  @abstractmethod
  def area(self):
    pass
  
  @abstractmethod
  def test(self):
    pass

class Rectangle(Shape):
  width, height = 5, 7

  def perimeter(self):
    return 2 * (self.width + self.height)

  def area(self):
    return self.width * self.height

class Circle(Shape):
  radius = 3

  def perimeter(self):
    return 2 * np.pi * self.radius

  def area(self):
    return np.pi * self.radius ** 2

breakpoint()

rec = Rectangle()
cir = Circle()

print(rec.perimeter())
print(cir.area())
```

에러 메시지 `TypeError: Can't instantiate abstract class Rectangle with abstract method test`. 없으니까 없다고 나온다. abstract class를 이용해서 내 코드 점검할 수 있는 것이 목적인듯.

#### source
- https://www.scaler.com/topics/python/data-abstraction-in-python/

### Unittest, Test-Driven Development
코드를 짰을 때 이것이 제대로 작동할지에 대한 검증/테스트하는 코드 짜기. 본 프로그램과 별도로 작성된 파일을 사용.

```python
# main.py
def hello_world(*args):
  'Add'
  return sum([arg for arg in args]) if len(args) else None
```

```python
# test_main.py
from unittest import TestCase
from main import *

class MyTest(TestCase):
  def test_hello_world(self):
    self.assertEqual(hello_world(), 'Hello, World!')
```
`python -m unittest` 입력 시 에러 발생. `hello_world()`에 전혀 연관성 없는 내용 넣음.

### 디자인 패턴
- to-do

### 도움 될 만한 자료
- https://available-stem-0cc.notion.site/CodeStates-b64f0788d8cf4151ab12f9e0dbd314d8
- https://github.com/dev-marko/clean-code-book