---
layout: post
related_posts:
    - /frontend/python
title:  "컴프리헨션 Comprehension(List, Set, Dictionary)"
date:   2025-09-11
categories:
  - frontend
  - python
description: >
  예시를 통해 파이썬 컴프리헨션(List, Set, Dict)을 정리
---
* toc
{:toc .large-only}

# 컴프리헨션(Comprehension)
컴프리헨션(Comprehension)은 파이썬에서 리스트, 세트, 딕셔너리 등의 컬렉션을 간단하게 생성하거나 변형하는 방법이다. 컴프리헨션은 반복문과 조건문을 사용하여 간결하게 컬렉션을 생성하는 기법으로, **간결하고 가독성 좋은 코드**를 작성할 수 있다.
## 리스트 컴프리헨션
리스트 컴프리헨션은 반복문을 사용하여 새로운 리스트를 만드는 방법이다.
### 기본문법
```python
[표현식 for 변수 in 반복가능객체 if 조건식]
```
### 예시 - 제곱수 리스트
```python
squares = [x**2 for x in range(5)]
print(squares)  # [0, 1, 4, 9, 16]
```
### 예시 - 짝수만 필터링
```python
evens = [x for x in range(10) if x % 2 == 0]
print(evens)  # [0, 2, 4, 6, 8]
```
## 세트 컴프리헨션
세트 컴프리헨션은 리스트 컴프리헨션과 거의 같지만, **중복을 허용하지 않고** 집합 자료형을 만든다.
### 예시 - 모음 추출
```python
text = "hello python"
vowels = {ch for ch in text if ch in "aeiou"}
print(vowels) # {'o', 'e'}
```
## 딕셔너리 컴프리헨션
딕셔너리 컴프리헨션은 반복문을 사용하여 키-값 쌍을 생성하는 방법이다.
### 기본 문법
```python
{key_expr: value_expr for 변수 in 반복가능객체 if 조건식}
```
### 예시 - 숫자:제곱값 매핑
```python
squares = {x: x**2 for x in range(5)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
### 예시 - 문자:아스키코드 매핑
```python
chars = {ch: ord(ch) for ch in "abc"}
print(chars)  # {'a': 97, 'b': 98, 'c': 99}
```