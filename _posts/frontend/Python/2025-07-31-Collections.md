---
layout: post
related_posts:
    - /frontend/python
title:  "파이썬 컬렉션 자료형 정리(List, Tuple, dictionary, Set)"
date:   2025-07-31
categories:
  - frontend
  - python
description: >
  파이썬의 기본 컬렉션 자료형(리스트, 튜플, 딕셔너리, 집합)에 대해 정리
---
* toc
{:toc .large-only}

# 컬렉션 타입이란?
* 여러 개의 데이터 항목을 하나의 단위로 관리할 수 있게 해주는 데이터 구조를 의미한다. 
* 파이썬에서는 리스트(List), 튜플(Tuple), 집합(Set), 딕셔너리(Dictionary) 등이 대표적인 컬렉션 타입에 속한다.

### 파이썬 대표적인 컬렉션
* **리스트(List)**: 여러 데이터를 순서대로 저장, `[]`
* **튜플(Tuple)**: 수정 불가능한 리스트, `()`  
* **딕셔너리(Dictionary)**: `key : value` 형태로 저장, `{}`
* **집합(Set)**: 중복을 허용하지 않는 자료 구조

### 변수와 컬렉션의 차이
* 변수: 오직 하나의 데이터만 저장  
* 컬렉션: 여러 데이터를 하나의 변수에 저장

# List `[]`
* **수정이 가능(mutable, 동적 개념)**
* 여러 데이터 타입을 함께 저장 가능
* 2차원 리스트도 지원

## PyObject_HEAD
![list](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FrbbGS%2FbtsNlJ3pII6%2FAAAAAAAAAAAAAAAAAAAAADZ_4vWsTlsb7UDjjXvI68BXsS1bijEAnOMx_1PO2yF8%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3D1JlxIGO35BdjtPMnwOSGbGwqy0I%253D)

* `PyObject_HEAD`는 `C언어`로 구현된 `파이썬 인터프리터(CPython)` 의 `구조체(struct)`에 포함된 매크로이다.[Python 리스트의 메모리 동작 상세 설명]()
* 파이썬 객체라면 반드시 가져야 하는 공통 필드를 정의 합니다.

```c
typedef struct {
    PyObject_HEAD   // 공통 필드: 참조 횟수, 타입 정보 포함!
    Py_ssize_t ob_size;  // 리스트의 길이
    PyObject **ob_item;  // 실제 아이템 배열 포인터
    Py_ssize_t allocated; // 할당된 배열 용량
} PyListObject;
```

## 리스트 생성
* 일반적으로 리스트(배열)은 오직 같은 데이터 타입만을 가져야 하지만, 파이썬에서는 **데이터 타입을 가리지 않는다.**
* 리스트는 대괄호 [ ]를 사용하여 생성하며, 
* 내부에 포함된 각 항목들은 쉼표로 구분된다.
```python
a = [] # 빈 리스트 생성
b = [10, 20] # 정수형 타입만 선언
c = ['파이썬', '자바']
d = [ 10, 20, '파이썬' ] # 정수, 실수, 문자열, 문자
li = [1, 2.5, 'A'], [2, 2.5 'B'];
```

## 패킹(Packing) & 언패킹(Unpacking)
* **패킹** : 여러 값을 하나의 리스트로 묶는 것
* **언패킹** : 리스트의 요소들을 여러 변수에 나누어 할당하는 것          

```python
li = [10, 20, 30]
print(li)

# 언패킹
a, b, c = li
print(a)  # 10
print(b)  # 20
print(c)  # 30
```

## 인덱싱(Indexing)
리스트의 특정 위치 요소에 접근 (0부터 시작)
* 인덱싱은 리스트에서 '하나의 요소'를 꺼내는 것이다.
* 결과는 → 꺼낸 하나의 값(값 자체)이다.
* 대괄호로 감싸지지 않는다.

### 1차원인 경우
```python
li = [10, 20, 30, 40]

print(li)      # [10, 20, 30, 40]
print(li[2])   # 30
print(li[-1])  # 40 (뒤에서 첫 번째)
print(li[0] + li[-1]) # 30
```
### 2차원인 경우
```python
li = [1, 2, '파이썬', ['김사과', '오렌지']]
print(li)               # [1, 2, '파이썬', ['김사과', '오렌지']]
print(type(li))         # <class 'list'>
print(li[1])            # 2
print(type(li[1]))      # <class 'int'>
print(li[3])            # ['김사과', '오렌지']
print(type(li[3]))      # <class 'list'>
print(li2[3][1])        # 오렌지
print(type(li2[3][1]))  # <class 'str'>
```
### 3차원인 경우
```python
li = [1, 2, 3, ['김사과', '오렌지', '반하나', ['🍟','🌭','🥩','🍗']]]          
print(li)              # [1, 2, 3, ['김사과', '오렌지', '반하나', ['🍟', '🌭', '🥩', '🍗']]]
print(li[2])           # 3
print(li[-1])          # ['김사과', '오렌지', '반하나', ['🍟', '🌭', '🥩', '🍗']]
반하나
print(li[-1][-2])      # 반하나
print(li[-1][-1])      # ['🍟', '🌭', '🥩', '🍗']
print(li[-1][-1][-2])  # 🥩
print(li[-1][-1][-1])  # 🍗
```

## 슬라이싱(Slicing)
리스트의 **일부분을 잘라서 가져오거나 수정할 때** 사용한다.
### 규칙
* 리스트`[시작 인덱스: 출력하고자하는 인덱스 + 1]`
* `[start:end:step]` 을 지정하지 않으면 전체 리스트에서 해당 조건만 반영된다.
* `[start:end:step]` 형태로 데이터를 삽입하면 리스트의 길이가 변경된다.
* `리스트[index] = [값]` 형태로 쓰면 해당 위치에 리스트 자체가 들어가서 중첩 리스트가 된다.

### 추가 설명
* start: 어디서 시작할지 (포함)
* end: 어디까지 갈지 (끝 인덱스는 포함하지 않음)
* step: 몇 칸씩 이동할지    
```python
li = [10, 20, 30, 40, 50, 60, 70]

print(li[0:3])    # [10, 20, 30]
print(li[2:5])    # [30, 40, 50]
print(li[1:])     # [20, 30, 40, 50, 60, 70]
print(li[:4])     # [10, 20, 30, 40]
print(li[1:6:2])  # [20, 40, 60]
print(li[::-2])   # [70, 50, 30, 10]
print(li[::-1])   # [70, 60, 50, 40, 30, 20, 10]
```

### 같은 주소 참조 주의
```python
li1 = [10, 20, 30, 40, 50]

li2 = li1
print(li2)    # [10, 20, 30, 40, 50]
li2[0] = 100  
print(li1)    # [100, 20, 30, 40, 50]
print(li2)    # [100, 20, 30, 40, 50]
```

### 다차원일 경우
```python
li = [1, 2, 3, ['김사과', '오렌지', '반하나', ['🍟','🌭','🥩','🍗']]]
print(li[2:3])    # [3]
print(li[3])      # ['김사과', '오렌지', '반하나', ['🍟', '🌭', '🥩', '🍗']]
print(li[3][:2])  # ['김사과', '오렌지']
```

### 슬라이싱과 단일 인덱스 차이
```python
# 슬라이싱으로 교체 (길이가 변경됨)
li = [10, 20, 30]
li[1:2] = ['😁','😂','😎','😍']
print(li) # [10, '😁', '😂', '😎', '😍', 30]

# 단일 인덱스에 리스트를 삽입 (중첩 리스트가 들어감)
li = [10, 20, 30]
li[1] = ['😁','😂','😎','😍']
print(li) # [10, ['😁','😂','😎','😍'], 30]

# 슬라이싱 데이터 삭제
li = [10, 20, 30, 40, 50]
li[1:3] = [] # 빈 리스트를 슬라이싱을 통해 저장하면 해당 요소 삭제됨
print(li) # [10, 40, 50]

# 인덱스 [] 값으로 교체
li = [10, 20, 30, 40, 50]
li[1] = []
print(li) # [10, [], 30, 40, 50]

# 인덱스로 데이터 삭제(del 함수 이용)
li = [10, 20, 30, 40, 50]
del li[1]
print(li) # [10, 30, 40, 50]
```

## 슬라이싱을 활용한 리스트 출력
![slicing](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fcg3Iuv%2FbtsNAjqwL9V%2FAAAAAAAAAAAAAAAAAAAAAFj7FeW62TPkJyljHU7yEyjqX2-oNp2g_OXRZXLOuNcl%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DwSAxYmZQi%252FY5r7lOMgev0hKkL2s%253D)

> step이 음수인 경우, end의 기본값은 -1입니다. 하지만 리스트의 마지막 요소 역시 인덱스 -1이므로, 전체를 역순으로 출력할 때는 end를 생략하여 사용합니다.

### 슬리이싱을 활용해 출력
```python
li = [100, 50, 70, 60, 20]
# start: 0, end: 5, step: 1
print(li[:])
print(li[0:])
print(li[:5])
print(li[::])
print(li[0:5:1])
```

### 슬라이싱을 활용해 역순으로 출력
```python
li = [100, 50, 70, 60, 20]
# step이 -1일 경우 start의 기본값은 length-1, end의 기본값은 -1으로 설정
print(li[::-1])
print(li[-1::-1])
print(li[4::-1])
print(li[4:None:-1])
```

## 리스트 연산
```python
li1 = [10,20,30]
li2 = [40,50,60]

print(li1 + li2)  # [10, 20, 30, 40, 50, 60]
print(li2 + li1)  # [40, 50, 60, 10, 20, 30]
print(li1 * 3)    # [10, 20, 30, 10, 20, 30, 10, 20, 30]
```
### in 연산자
특정 값이 리스트에 포함되어 있는지 확인(다른 컬렉션 타입도 가능)
```python
li = [10, 20, 30, 40, 50]

# 값이 리스트에 있는지 확인
print(20 in li)   # True
print(60 in li)   # False

# 값이 리스트에 없는지 확인
print(60 not in li)  # True
print(30 not in li)  # False
```

## 리스트 항목 삭제
### 슬라이싱을 통한 삭제
```python
li4 = [10, 20, 30, 40, 50]
li4[1:3] = [] # 빈 리스트를 슬라이싱을 통해 저장하면 해당 요소가 삭제됨
print(li4)    # [10, 40, 50]
```
### 단일 인덱스에 빈 리스트 대입 (삭제 아님)
```python
li4 = [10, 20, 30, 40, 50]
li4[1] = []   # 삭제가 아니라 빈 리스트를 값으로 치환
print(li4)    # [10, [], 30, 40, 50]
```
### `del` 키워드로 삭제
```python
li4 = [10, 20, 30, 40, 50]
print(li4)
del li4[1]  # 해당 요소 완전히 삭제
print(li4)  # [10, 30, 40, 50]
```

## 함수와 메서드
### len()
객체의 길이를 반환하는 파이썬의 기본 내장 함수
```python
li = [10, 20, 30]
print(len(li))
```

### append()
리스트 끝에 새로운 요소를 추가
```python
li = [10, 20, 30]
li.append(40)
print(li)   # [10, 20, 30, 40]
```
### extend()
리스트 끝에 새로운 여러 요소를 추가
```python
li = [10, 20, 30]
# li.extend(40) # TypeError: 'int' object is not iterable
li.extend([40]) 
print(li)   # [10, 20, 30, 40]
li.extend([50, 60])
print(li)   # [10, 20, 30, 40, 50, 60]
```
### pop()
마지막으로 삽입된 요소를 삭제하고 삭제된 요소를 반환
```python
li = [10, 20, 30]
temp = li.pop()
print(temp)  # 30
print(li)   # [10, 20]
```
### index()
리스트에서 특정 값의 인덱스를 반환
```python
li = [10, 20, 30]
print(li.index(20))  # 1
# print(li.index(100)) # ValueError: 100 is not in list
```

### reverse()
리스트의 순서를 역순으로 정렬
```python
li = [30, 10, 20]
li.reverse()
print(li)   # [20, 10, 30]
```

### del()
삭제하는 메서드
```python
li = [10, 20, 30]
del li[1]
print(li)   # [10, 30]

del li
# NameError: name 'li' is not defined
```

### sort()
오름차순으로 정렬
```python
li = [30, 10, 20]
li.sort()
print(li)   # [10, 20, 30]
```
### sort(reverse=True)
내림차순으로 정렬
```python
li = [30, 10, 20]
li.sort(reverse=True)
print(li)   # [30, 20, 10]
```
### sorted()
정렬된 새로운 리스트 반환해주는 함수
```python
li = [30, 10, 20]
print(sorted(li)) # [10, 20, 30]
print(li)         # [30, 10, 20]
li = sorted(li)
print(li)         # [10, 20, 30]   
li = sorted(li, reverse=True)
print(li)         # [30, 20, 10]
```

### count()
리스트에서 특정 요소의 개수 반환
```python
li = [10, 20, 20, 30]
print(li.count(20))   # 2
print(li.count(100))  # 0
```

## 2차원 리스트
리스트 안에 리스트가 존재하는 구조`[[]]`
```python
li = [
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9, 10, 11, 12]
]

print(li[0][0])   # 1
print(li[0][1])   # 2
```

### 활용예시 및 코드 해석
```python
list1 = []; # 빈 리스트 생성
list2 = []; # 빈 리스트 생성
value = 1; # 정수형 변수 선언(1로 초기화)

for i in range(0, 3):
    for k in range(0, 4):
        list1.append(value);
        # [1, 2, 3, 4]
        # [5, 6, 7, 8]
        # [9, 10, 11, 12]
        value = value + 1; # 2, 3, 4, 5, 6, 7, 8, 9 ...
    list2.append(list1) # [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    list1 = [] # []

for i in range(0, 3):
    for k in rnage(0, 4):
        print("%3d" % list2[i][k], end = " ")
        # [0,0] [0,1] [0,2] [0,3] => 1 2 3 4
        # [1,0] [1,1] [1,2] [1,3] => 5 6 7 8
        # ...
    print("")
"""
  1   2   3   4
  5   6   7   8
  9  10  11  12
"""
```

# Tuble 튜플 `()`
* **수정이 불가능 (immutable, 정적 개념)**
* 여러 데이터 타입 저장 가능
* **순서가 있는 컬렉션(iterable한 컬렉션)**
* 괄호 `()`를 사용하여 생성하고, 항목들은 쉼표 `,`로 구분된다.
* 리스트(`[]`)와 비슷하지만, 생성 후 수정이 불가능 -> **읽기 전용**
  * 리스트와 달리 `append()`, `remove()` 같은 **수정 메서드가 없다.**
* 때문에 **잠금**을 위해 **리스트 -> 튜플**을 많이 사용한다.

### 튜플 기본
**튜플 초기화**

```python
tu = ()
print(tu)           # ()
print(type(tu))     # <class 'tuple'>
```
**요소가 1개일 경우 끝에 `,`를 꼭! 붙여야 함**

```python
tu = (1)
print(tu)           # 1
print(type(tu))     # <class 'int'>

tu = (1,)           # 요소 1개일 때는 반드시 콤마 붙여야 함
print(tu)           # (1,)
print(type(tu))     # <class 'tuple'>
```
**요소를 넣어서 생성**

```python
tu = (1, 3, 5, 6)
print(tu)           # (1, 3, 5, 6)
print(type(tu))     # <class 'tuple'>

tu = 1, 3, 5, 6     # 괄호 생략 가능
print(tu)           # (1, 3, 5, 6)
```
**리스트 <-> 튜플**
```python
tu = tuple([1, 3, 5, 6]) 
print(tu)           #  (1, 3, 5, 6)
print(type(tu))     # <class 'tuple'>

li = list(tu)
print(li)           # [1, 3, 5, 6]
print(type(li))     # <class 'list'>
```

### 인덱싱과 슬라이딩
**튜플은 불변(immutable) → 직접 수정 불가**
```python
tu = (10, 20)
print(tu)      # (10, 20)
tu.append(30)  # AttributeError: 'tuple' object has no attribute 'append'
```

```python
tu = ('apple', 'banana', ('🍎', '🍌'))

# tu[0] = 'orange'  # TypeError: 'tuple' object does not support item assignment
tu = 'orange'
print(tu)           # orange
```
**내부에 리스트가 있으면 그 리스트는 수정 가능**

```python
tu = ('apple', 'banana', ['🍎', '🍌'])
print(type(tu[2]))  # <class 'list'>
tu[2][0] = '🍍'
print(tu)           # ('apple', 'banana', ['🍍', '🍌'])
```
**튜플의 슬라이싱은 결과도 튜플로 반환**

```python
tu = (1, 2, 'apple', 'banana')
print(tu[1:])       # (2, 'apple', 'banana')
print(tu[1:3])      # (2, 'apple')
```
### 연산
```python
tu1 = (10, 20, 30)
tu2 = (40, 50, 60)

print(tu1 + tu2)    # (10, 20, 30, 40, 50, 60)
print(tu2 + tu1)    # (40, 50, 60, 10, 20, 30)

tu1 += (40, 50, 60) 
print(tu1)          # (10, 20, 30, 40, 50, 60)

print(tu1 * 3)      # (10, 20, 30, 40, 50, 60, 10, 20, 30, 40, 50, 60, 10, 20, 30, 40, 50, 60)
```

### 정렬
* 튜플은 **불변(immutable)** → **sort() 사용 불가**
* **대신 `sorted()` 사용**

```python
tu = (10, 30, 100, 90, 50)
# tu.sort()  # AttributeError: 'tuple' object has no attribute 'sort'
tu = sorted(tu)         # sorted의 반환 타입은 리스트
print(tu)               # [10, 30, 50, 90, 100]
tu = tuple(tu)          # (10, 30, 100, 90, 50)
print(sorted(tu, reverse=True)) # [100, 90, 50, 30, 10]
```

# Dictionary 딕셔너리 `{}` `key : value`
* **키-값 쌍 저장 (key : value)**
* **수정이 가능(mutable, 동적 개념)**
* **키는 불변(immutable) 타입 & 중복 X, 값은 어떤 타입이든 가능 & 중복 가능**
* 파이썬 3.7 이전까지는 순서 없음 
* 3.7 이후는 입력 순서를 유지하지만 "논리적으로는" 순서 없음으로 간주
* 딕셔너리는 중괄호 `{}`를 사용하여 생성하고, 키-값 쌍들은 쉼표 `,`로 구분된다. 
* 각 키-값 쌍은 콜론 `:`으로 구분된다

## 딕셔너리 기본
**딕셔너리 초기화**

```python
dic = {}
print(dic)        # {}
print(type(dic))  # <class 'dict'>
```
**딕셔너리 키-값**

```python
dic = {1:'김사과', 2:'반하나', 3:'오렌지', 4:'이메론'}
print(dic)       # {1: '김사과', 2: '반하나', 3: '오렌지', 4: '이메론'}
print(type(dic)) # <class 'dict'>
```

**딕셔너리 키로 값을 가져오는 방법**
```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}
print(dic)            # {'no': 1, 'userid': 'apple', 'name': '김사과', 'hp': '010-1111-1111'}
print(dic['userid'])  # apple
print(dic['hp'])      # 010-1111-1111
```

## 딕셔너리 수정
* 딕셔너리에 키-값 쌍을 추가하거나 제거하거나, 기존의 키의 값을 변경할 수 있다. 
* 딕셔너리의 키는 변경 불가능한(immutable) 타입이어야 한다. 
* 예를 들어, 문자열, 정수, 튜플은 딕셔너리의 키로 사용할 수 있지만, 
* 리스트는 딕셔너리의 키로 사용할 수 없다. 
* 하지만 딕셔너리의 값은 어떤 타입이든 상관없습니다.

**새로운 key, value 추가하는 방법**

```python
student = {'학번' : 100, '이름' : '홍길동', '학과' : '마술학과'}
student['전화번호'] = '010-1234-5678'
print(student)    # {'학번' : 100, '이름' : '홍길동', '학과' : '마술학과', '전화번호' : '010-1234-5678'}
```

**딕셔너리 키 값은 immutable 타입**

```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}

# dic[[10, 20, 30]] = ['십', '이십', '삼십'] # TypeError: unhashable type: 'list'
dic[(10, 20, 30)] = ['십', '이십', '삼십']
print(dic) # {'no': 1, 'userid': 'apple', 'name': '김사과', 'hp': '010-1111-1111',(10, 20, 30): ['십', '이십', '삼십']}

dic['과일'] = {'사과':'🍎', '딸기':'🍓', '수박':'🍉'}
print(dic3) # {'no': 1, 'userid': 'apple', 'name': '김사과', 'hp': '010-1111-1111',(10, 20, 30): ['십', '이십', '삼십'], '과일': {'사과': '🍎', '딸기': '🍓', '수박': '🍉'}}
```

## 함수와 메서드
### keys()
딕셔너리의 모든 키를 반환
```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}
print(dic.keys()) # dict_keys(['no', 'userid', 'name', 'hp'])
```

### values()
딕셔너리의 모든 값을 반환
```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}
print(dic.values()) # dict_values([1, 'apple', '김사과', '010-1111-1111'])
```

### items()
딕셔너리의 모든 키-값을 튜플로 반환
```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}
print(dic.items()) # dict_items([('no', 1), ('userid', 'apple'), ('name', '김사과'), ('hp', '010-1111-1111')])
```

### get()
* 특정 키에 대한 값을 반환. 
* 만약 키가 딕셔너리에 없으면 None을 반환   
* None을 치환할 수 있는 문자열을 설정할 수 있음

```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}

print(dic['userid'])     # apple
# print(dic['gender'])   # KeyError: 'gender'
print(dic.get('userid')) # apple
print(dic.get('gender', '성별 알 수 없음')) # 성별 알 수 없음
print(dic.get('name', '이름 알 수 없음'))   # 김사과
```
### pop()
* 특정 키에 대한 값을 제거하고 제거된 값을 반환. 
* 키가 없다면 에러

```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}

temp = dic.pop('hp')
print(dic)  # {'no': 1, 'userid': 'apple', 'name': '김사과'}
print(temp) # 010-1111-1111
```
### in
딕셔너리에 특정 키가 있는지 확인
```python
dic = {'no':1, 'userid':'apple', 'name':'김사과', 'hp':'010-1111-1111'}

print('hp' in dic)  # True
print('010-1111-1111' in dic) # False
```

# Set `{}`, `set()`
* 중복 없는 데이터
* 순서가 없는 컬렉션
* 중괄호 `{}`를 사용하여 생성하거나, `set()` 생성자를 사용할 수 있다.
* **수정이 가능(mutable, 동적 개념)**

### Set 기본
```python
s = {}         
print(s)        # {}
print(type(s))  # <class 'dict'>
```

```python
s = {1, 3, 5, 7}
print(s)        # {1, 3, 5, 7}
print(type(s))  # <class 'set'>
```
**형 변환**

```python
li = [1, 2, 3, 4]
print(type(li))
s4 = set(li)
print(type(s))
```
**중복 제거**

```python
s = {1, 3, 5, 3, 7, 9, 1, 5, 10, 7}
print(s)        # {1, 3, 5, 7, 9, 10}

li = [1, 3, 5, 3, 7, 9, 1, 5, 10, 7]
print(li)       # [1, 3, 5, 3, 7, 9, 1, 5, 10, 7]
s = set(li)
print(list(s))  # [1, 3, 5, 7, 9, 10]
```

## 함수와 메서드
### add()
세트의 요소를 추가
```python
s1 = {1, 3, 5, 7}
s1.add(2)
print(s1) # {1, 2, 3, 5, 7}
```

### update()
세트에 여러 요소를 추가
```python
s1 = {1, 3, 5, 7}
s1.update([2, 4, 6, 8, 10])
print(s1) # {1, 2, 3, 4, 5, 6, 7, 8, 10}
```

### remove()
세트의 요소를 제거.
단, 해당 요소가 없으면 `KeyError` 발생.
```python
s1 = {1, 3, 5, 7}
s1.remove(3)
print(s1)  # {1, 5, 7}

# 없는 값 제거 시 에러 발생
# s1.remove(3)  # KeyError: 3
```

### discard()
세트의 요소를 제거.
단, **요소가 없어도 에러 발생하지 않음.**
```python
s1 = {1, 3, 5, 7}
s1.discard(3)
print(s1)  # {1, 5, 7}

s1.discard(3)  # 이미 없어도 에러 X
print(s1)  # {1, 5, 7}
```

### copy()
세트를 복사 (깊은 복사 vs 얕은 복사 비교)
```python
s1 = {1, 3, 5, 7}

# 얕은 복사: 메모리 주소만 공유
s2 = s1         # 같은 주소
print(id(s1))   # 139616806402560
print(id(s2))   # 139616806402560

# 깊은 복사: 새로운 객체 생성
s2 = s1.copy()  # 다른 주소
print(s1)       # {1, 3, 5, 7}
print(s2)       # {1, 3, 5, 7}
print(id(s1))   # 139616806402560
print(id(s2))   # 139616806401888
```

## 집합(Set) 연산
### union() - 합집합
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.union(s4)
print(result1)  # {10, 20, 30, 40, 50, 60, 70}

result2 = s3 | s4
print(result2)  # {10, 20, 30, 40, 50, 60, 70}
```

### intersecton() - 교집합
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.intersection(s4)
print(result1)  # {40, 50, 30}

result2 = s3 & s4
print(result2)  # {40, 50, 30}
```

### difference() - 차집합
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.difference(s4)
print(result1)  # {10, 20}

result2 = s3 - s4
print(result2)  # {10, 20}
```

### symmetric_difference() - 대칭 차집합
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.symmetric_difference(s4)
print(result1)  # {20, 70, 10, 60}

result2 = s3 ^ s4
print(result2)  # {20, 70, 10, 60}
```