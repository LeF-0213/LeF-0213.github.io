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

* `PyObject_HEAD`는 `C언어`로 구현된 `파이썬 인터프리터(CPython)` 의 `구조체(struct)`에 포함된 매크로이다.
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
libers = [10, 20, 30]
print(libers)

# 언패킹
a, b, c = libers
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
li[1:2] = [40, 50]
print(li) # [10, 40, 50, 30]

# 단일 인덱스에 리스트를 삽입 (중첩 리스트가 들어감)
li = [10, 20, 30]
li[1] = [40, 50]
print(li) # [10, [40, 50], 30]
```

### 슬라이싱을 활용해 역순으로 정렬
```python
li7 = [100, 50, 70, 60, 20]
print(li7[::-1])
# step이 -1일 경우 start의 기본값은 length-1, end의 기본값은 -1으로 설정
print(li7[-1::-1])
print(li7[4::-1])
print(li7[4:None:-1])
```

## 리스트 연산
```python
li = [10,20,30]
li1 = [40,50,60]

print(li + li1)   # [10, 20, 30, 40, 50, 60]
print(li * 3)      # [10, 20, 30, 10, 20, 30, 10, 20, 30]
```
### in 연산자
특정 값이 리스트에 포함되어 있는지 확인
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
### append()
리스트 끝에 새로운 요소를 추가
```python
myList = [10, 20, 30]
myList.append(40)
print(myList)   # [10, 20, 30, 40]
```
### extend()
리스트 끝에 새로운 여러 요소를 추가
```python
myList = [10, 20, 30]
myList.extend([40, 50])
print(myList)   # [10, 20, 30, 40, 50]
```
### pop()
마지막으로 삽입된 데이터를 삭제하고 삭제된 요소를 반환
```python
myList = [10, 20, 30]
removed = myList.pop()
print(removed)  # 30
print(myList)   # [10, 20]
```
### index()
리스트에서 특정 값의 인덱스를 반환
```python
myList = [10, 20, 30]
print(myList.index(20))  # 1
```

### reverse()
리스트의 순서를 역순으로 정렬
```python
myList = [30, 10, 20]
myList.reverse()
print(myList)   # [20, 10, 30]
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
myList = [30, 10, 20]
myList.sort()
print(myList)   # [10, 20, 30]
```
### sort(reverse=True)
내림차순으로 정렬
```python
myList = [30, 10, 20]
myList.sort(reverse=True)
print(myList)   # [30, 20, 10]
```
### sorted()
정렬된 새로운 리스트 반환
```python
myList = [30, 10, 20]
print(sorted(myList))        # [10, 20, 30]
print(sorted(myList, reverse=True))  # [30, 20, 10]
```

### count()
특정 값 개수 반환
```python
myList = [10, 20, 20, 30]
print(myList.count(20))   # 2
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
* 리스트(`[]`)와 비슷하지만, 생성 후 수정기 불가능 -> **읽기 전용**
### 튜플 기본
```python
li = (10, 20)
li
# (10, 20)
li.append(30)  # AttributeError: 'tuple' object has no attribute 'append'
```
* 리스트와 달리 `append()`, `remove()` 같은 **수정 메서드가 없다.**
* 생성 시 `()` 소괄호를 사용한다.
```python
tu1 = ()
print(tu1)            # ()
print(type(tu1))      # <class 'tuple'>

tu2 = (1,)            # 요소 1개일 때는 반드시 콤마 붙여야 함
print(tu2)            # (1,)
print(type(tu2))      # <class 'tuple'>

tu3 = 1, 3, 5, 7      # 괄호 생략 가능
print(tu3)            # (1, 3, 5, 7)
```
### 튜플 수정
```python
tu5 = ('apple', 'banana', ('🍎', '🍌'))

# 튜플은 불변(immutable) → 직접 수정 불가
# tu5[0] = 'orange'  # TypeError 발생

# 내부에 리스트가 있으면 그 리스트는 수정 가능
tu6 = ('apple', 'banana', ['🍎', '🍌'])
tu6[2][0] = '❤'
print(tu6)  # ('apple', 'banana', ['❤', '🍌'])
```
### 정렬
튜플은 불변(immutable) → **sort() 사용 불가**
**대신 `sorted()` 사용**
```python
tu = (10, 30, 100, 90, 50)
print(sorted(tu))              # [10, 30, 50, 90, 100]
print(sorted(tu, reverse=True))# [100, 90, 50, 30, 10]
```

# Dictionary 딕셔너리 `{}` `key : value`
* 키-값 쌍 저장 (key : value)
* **수정이 가능(mutable, 동적 개념)**
* 키는 불변(immutable)타입, 값은 어떤 타입이든 가능
* 저장하는 순서가 없음
### 새로운 key, value 추가하는 방법
```python
student = {'학번' : 100, '이름' : '홍길동', '학과' : '마술학과'}
student['전화번호'] = '010-1234-5678'
```
## 함수와 메서드
### keys()
딕셔너리의 모든 키를 반환
```python
print(student.keys()) # dict_keys(['학번', '이름', '학과', '전화번호'])
print(list(student.keys())) # ['학번', '이름', '학과', '전화번호']
```
### values()
딕셔너리의 모든 값을 반환
```python
print(student.values()) # dict_values([1, '홍길동', '미술학과', '01012345678'])
```
### items()
딕셔너리의 모든 키-값을 튜플로 반환
```python
print(student.items())    # dict_items([('학번', 100), ('이름', '홍길동'), ('학과', '미술학과'), ('전화번호', '010-1234-5678')])
```
### get()
특정 키에 대한 값을 반환. 만약 키가 딕셔너리에 없으면 None을 반환   
None을 치환할 수 있는 문자열을 설정할 수 있음
```python
print(student.get('이름')) # '홍길동'
print(student['이름']) # '홍길동'
# print(student['gender']) # KeyError: 'gender'
print(student.get('gender')) # None
print(student.get('gender', '성별 알수없음')) # '성별 알수없음'
```
### pop()
특정 키에 대한 값을 제거하고 제거된 값을 반환. 키가 없다면 에러
```python
removed = student.pop('학과')
print(removed)   # 미술학과
print(student)   # {'학번': 100, '이름': '홍길동'}
```
### in
딕셔너리에 특정 키가 있는지 확인
```python
'학번' in student # True
'주소' in student # False
```

# Set `{}`, `set()`
* 중복 없는 데이터
* * **수정이 가능(mutable, 동적 개념)**
```python
st = {1,2,3,2}
print(st)    # {1,2,3}
```
## set 생성
```python
st = {}
st = set()
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
s2 = s1
print(id(s1))  
print(id(s2))  # 같은 주소

# 깊은 복사: 새로운 객체 생성
s2 = s1.copy()
print(s1)  # {1, 3, 5, 7}
print(s2)  # {1, 3, 5, 7}
print(id(s1))  
print(id(s2))  # 다른 주소
```
## 집합 연산
### union() (합집합)
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.union(s4)
print(result1)  # {10, 20, 30, 40, 50, 60, 70}

result2 = s3 | s4
print(result2)  # {10, 20, 30, 40, 50, 60, 70}
```
### intersecton() (교집합)
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.intersection(s4)
print(result1)  # {40, 50, 30}

result2 = s3 & s4
print(result2)  # {40, 50, 30}
```
### difference() (차집합)
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.difference(s4)
print(result1)  # {10, 20}

result2 = s3 - s4
print(result2)  # {10, 20}
```
### symmetric_difference() (대칭 차집합)
```python
s3 = {10, 20, 30, 40, 50}
s4 = {30, 40, 50, 60, 70}

result1 = s3.symmetric_difference(s4)
print(result1)  # {20, 70, 10, 60}

result2 = s3 ^ s4
print(result2)  # {20, 70, 10, 60}
```