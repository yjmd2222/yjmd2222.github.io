---
title:  "AIB Section 3 딥러닝 Sprint 0"
excerpt: "python class에 대해 배워보자"
header:
  teaser: /assets/images/s3dl00-class.png
categories:
  - AIB_Section_3
tags:
  - AIB
  - class
  - oop
date: 2023-04-17 19:00
last_modified_at: 2023-04-17T19:00:00-09:00
---

## Class

파이썬에서의 모든 것은 객체라고 볼 수 있다. 이제까지 사용했던 `str`의 instance, `pandas.DataFrame()`, 또는 사용자 지정 함수 전부 다 객체로 간주한다. 그래서 `return func`같은 것으로 함수를 객체로 간주하여 반환하고, 이 반환된 값을 `()`로 실행할 수도 있던 것 경험했음. 클래스를 만들면 클래스의 instance를 이용할 수 있다. 각각 하나의 instance가 객체.

객체: 값과 행동을 묶는다. 예를 들어 `list`:
```python
x = [1,2,3] # 객체. 값들이 있음. 각 element를 attribute라고 할 수 있다.
x.append(4) # 행동으로 4를 element로 추가함. method에 해당.
```

### instance, attribute, method

클래스는 정의를 하고 사용하기 위해서 instance를 만든다. `str`, `list`는 내장. `''`, `[]` 등으로 instance를 만들 수 있다. 내가 만든 instance는 `instance_1 = my_class()` 등으로 정의한다.

`instance_1.attribute_1`으로 attribute을 볼 수 있고, `instance_1.method_1()`으로 method 실행 가능.

### 클래스 정의 및 instance / method

```python
class Dog:
  def __init__(self, breed, age): # instance를 어떻게 만들지
    self.breed = breed
    self.age = age

  def sound(self, sound): # method
    print(self.breed, 'barks', sound*5)

  def how_old(self):
    print(f'{self.breed}는 {self.age}살')

maltese = Dog('maltese', 3)
maltese.sound('왕')
```

### 상속

parent class의 내용을 받아 child class가 사용할 수 있고, 일부를 변경할 수도 있다.

```python
class Cat(Dog):
  def __init__(self, breed, age, whiskers):
    super().__init__(breed, age)
    self.whiskers = whiskers

  def sound(self, sound):
    super().sound(sound)
    print('고양이는 짖지 않음')
    print(self.breed, 'meows', sound*5)

rb = Cat('Russian Blue', 2, 5)
rb.sound('애용')
```