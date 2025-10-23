---
layout: post
related_posts:
    - /frontend/python
title:  "함수와 변수"
date:   2025-08-05
categories:
  - frontend
  - python
description: >
  함수의 정의와 종류, 활용방식
---
* toc
{:toc .large-only}

# 변수란?
* **단일 데이터**를 담는 이름표 역할
* 변수에 값을 할당하면, 그 값이 메모리에 저장되고 변수 이름은 그 값을 가리키게 된다.
* 변수를 선언하면 반드시 **초기화** 후 사용해야 한다.

### 변수 이름 규칙
* 변수 이름은 알파벳(대문자 또는 소문자), 숫자, 밑줄(_)로 구성될 수 있다.
* 숫자로 시작할 수 없다.
* 공백을 포함할 수 없다. 대신 밑줄(_)을 사용할 수 있다
* 파이썬의 예약어(키워드)는 변수 이름으로 사용할 수 없다. 
* 예약어는 if, for, class, def 등 파이썬이 이미 사용하는 특별한 단어들이다.

```python
# 올바른 변수명
name = "김사과"
_name = "김사과"
name_1 = "김사과"
name2 = "김사과"
myName = "김사과"  # 카멜 케이스
my_name = "김사과"  # 스네이크 케이스 (파이썬 권장)

# 잘못된 변수명
2name = "김사과"      # 숫자로 시작
my-name = "김사과"    # 하이픈 사용
for = 10             # 예약어 사용
my name = "김사과"    # 공백 포함
```

### 변수 선언 및 초기화
* 파이썬은 동적 타입 언어이기 때문에, 변수를 선언할 때 자료형을 명시하지 않아도 되고,
* 한 번 정해진 변수에 다른 자료형의 값을 다시 저장할 수 있다.
* 기본자료형 초기화
  * **정수형(int)** → 0
  * **실수형(float)** → 0.0
  * **문자형(str, 한 글자 문자열)** → '' (빈 문자열)
  * **문자열(str)** → "" 또는 None
  * **불린형(bool)** -> False 또는 True
    ```python
    number = 0        # 정수 초기화
    pi = 0.0          # 실수 초기화
    letter = ''       # 문자 초기화 (파이썬은 char 자료형이 없고 str 사용) ('', "" 둘다 사용 가능)
    text = None       # 문자열 초기화 (아직 값 없음)
    isTrue = False    # 불린형 초기화

    # type(): 파이썬에서 변수나 값의 데이터 타입을 확인하는 데 사용되는 내장 함수
    print(type(number)) # <class 'int'>
    print(type(pi))      # <class 'float'>
    print(type(letter))  # <class 'str'>
    ```

## 변수와 메모리
Python에서 **"모든 것이 객체"**이고, **"변수는 참조"**이다.
![DataArea](https://github.com/user-attachments/assets/f1f9ed40-d6d8-41b2-8a56-c27a0ada3f94)

### stack (스택)
* 변수명만 저장된다.
* 실제 값이 아닌 **메모리 주소(참조)**를 가리킨다.
* name, age, items 같은 변수명들이 여기에 위치한다.

### Heap (힙)
* 실제 데이터 객체가 저장됩니다
* Python에서는 모든 것이 객체이므로
    * 문자열 "김사과" → str 객체
    * 숫자 20 → int 객체
    * 리스트 [1, 2, 3] → list 객체
    * 딕셔너리 → dict 객체

### 동작 방식
* 변수 선언 시 Stack에 변수명 생성
* Heap에 실제 데이터 객체 생성
* Stack의 변수가 Heap의 객체 주소를 참조 (화살표)

### 리터럴 공유(객체 캐싱)
* 리터럴 공유는 동일한 값을 가진 불변 객체가 여러 곳에서 사용될 때, 
* 새로운 객체를 만들지 않고 기존 객체를 재사용하는 방식이다. 
* 파이썬은 **리터럴 캐싱**으로 동일한 불변 객체를 재사용한다.
* 이를 통해 파이썬은 메모리 사용을 최적화할 수 있습니다.

![literalCasing](https://github.com/user-attachments/assets/76f9885c-a554-4ccc-b096-f3a7765f19d8)
#### 캐싱되는 경우
* **같은 메모리 주소 공유** 
* **작은 정수(-5 ~ 256)**: Python이 미리 캐싱
* **짧은 문자열**: 자동으로 intern되어 캐싱

#### 캐싱되지 않는 경우
* **각각 새로운 객체 생성**
* **큰 정수(257이상)**: 매번 새 객체 생성
* **가변 객체 (list, dict)**: 절대 캐싱 안 됨

#### 캐싱 차이가 생기는 이유
* **메모리 효율**: 자주 쓰는 작은 값들은 미리 만들어두고 재사용
* **성능 최적화**: 매번 새로 만들지 않아 속도 향상
* **가변성 문제**: list 같은 가변 객체는 독립성 보장을 위해 항상 새로 생성

```python
a = 200
b = 200

# is: 파이썬에서 객체의 동일성을 비교하는 연산자
print(a is b) # True
# id(): 파이썬의 내장 함수로, 주어진 객체의 고유한 메모리 주소를 반환
print(id(a)) # 11660744
print(id(b)) # 11660744

a = 'Hello Python'
b = 'Hello Python'

print(a is b) # False
```
> 200이 객체인 이유:        
> type, reference count, value 등의 정보가 함께 저장
<table>
  <tr>
    <td><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FK0qvq%2FbtsJ92lILiU%2FAAAAAAAAAAAAAAAAAAAAAKlcnobaHbVbuoFM2UP7xpNKbvt7DWKFefuUVfZwyCBa%2Fimg.webp%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DoeMXZamKoOF284P2cZMuZ8ykRhI%253D" alt="LiteralCasing1" width="400"></td>
    <td><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbnojEK%2FbtsJ9njyXf2%2FAAAAAAAAAAAAAAAAAAAAAADUZKeupRfBbZWCOOT_JVOURYE2KlaiCeXC3M4srYSy%2Fimg.webp%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DC6VexxEJr8YlEAXNvXYIXh009qo%253D" alt="LiteralCasing2" width="400"></td>
  </tr>
</table>

## 불변 객체와 가변 객체
### 불변 객체(Immutable Object)
* 한 번 생성되면 값을 변경할 수 없는 객체이다.
* 변경하려고 하면 새로운 객체가 생성된다.
* **종류**: int, float, str, tuple, frozenset

```python
x = 10
print(id(x)) # 11654664

x = x + 1
print(id(x)) # 11654696
```
* CPU가 x + 1을 다시 계산한 값을 다시 heap에 전달
* x가 x + 1이 계산된 값을 참조함
* 파이썬은 heap에 자료형이 올라가고, 자바는 stack에 올라감
파이썬은 모든 데이터가 참조형

#### Java의 경우 불변 객체
```java
int age = 20;
```
* 자바는 stack에 int에 할당하는 4바이트를 먼저 주고 20을 집어넣음
* 자바는 참조형을 힙에 넣어 사용함

### 가변 객체(Mutable Object)
* 값이 변경되어도 메모리 주소가 유지되는 객체이다.
* 객체 내부의 내용만 변경된다.
* **종류**: list, dict, set

```python
# 리스트는 가변 객체
my_list = [1, 2, 3]
print(id(my_list))  # 140234567890

my_list.append(4)
print(id(my_list))  # 140234567890 (같은 주소!)
print(my_list)      # [1, 2, 3, 4]

# 딕셔너리도 가변 객체
my_dict = {'name': '김사과'}
print(id(my_dict))  # 140234567920

my_dict['age'] = 20
print(id(my_dict))  # 140234567920 (같은 주소!)
```

## 얕은 복사와 깊은 복사
### 얕은 복사 (Shallow Copy)
* 객체의 최상위 레벨만 복사
* 내부의 중첩된 객체는 참조만 복사됨

```python
# 얕은 복사 문제점
list1 = [1, 2, [3, 4]]
list2 = list1

list2[0] = 99
print(list1) # [1, 2, [3, 4]]
```

## 자료형 변환하기
* 파이썬에서는 서로 다른 자료형 간에 변환할 수 있다.
* 이를 형 변환(Type Casting)이라고 한다.

### 정수형 <-> 실수형
```python
# 정수형 → 실수형
a = 10
b = float(a)  # 10.0

# 실수형 → 정수형 (소수점 버림)
c = 3.14
d = int(c)    # 3
```
### 문자형 <-> 정수형, 실수형
```python
# 문자형 → 정수형, 실수형
e = "100"
f = int(e)    # 100
g = float(e)  # 100.0

# 정수형/실수형 → 문자형
h = 42
i = str(h)    # "42"

j = 3.14
k = str(j)    # "3.14"
```

### 자동형변환(동적타이핑)
* 파이썬은 변수 선언 시 자료형을 지정하지 않아도 되고,
* 실행 중에 자동으로 형이 결정된다.

```python
  a = 10;
  a = 10.5;
  a = 'A'
```

## 변수 삭제하기
* 변수를 삭제하기 위해 del문을 사용한다.
* del문은 지정된 변수를 제거하고 해당 메모리 공간을 해제한다.
* 변수가 삭제되면 해당 이름으로 변수에 더 이상 접근할 수 없다.
* 주의해야 할 점은 del문을 사용하여 변수를 삭제할 때 해당 변수에 연결된 메모리가 해제되지만, 변수가 참조하던 값 자체는 삭제되지 않는다.[가비지 컬렉션 참조](https://lef-0213.github.io/frontend/python/2025-10-21-garbage-collection/)

```python
isTrue = False
print(isTrue) # False
del isTrue
print(isTrue) # NameError: name 'isTrue' is not defined
```

# 함수란?
**데이터를 입력받아 처리하고 결과를 반환하는 기능**을 이야기한다.    
![function](https://thebook.io/img/080398/039.jpg)

## 함수의 종류
함수는 **매개변수**와 **반환값**의 유무에 따라 4가지 형태로 나뉜다.

### 1. 매개변수 O, 반환값 O

```python
def add(a, b):
  return a + b

result = add(3, 5)
print(result) # 8
```
### 2. 매개변수 O, 반환값 X

```python
def print_sum(a, b):
  print(f"합계: {a + b}")

print_sum(3, 5) # 합계: 8
```
### 3. 매개변수 X, 반환값 O

```python
def get_pi():
    return 3.14

pi = get_pi()
print(pi)  # 3.14
```
### 4. 매개변수 X, 반환값 X

```python
def greet():
  print("Hello, World!")

grett() # Hello, World!
```

### 프로그램 코딩 방식 1(단순 순차적 코딩)
1. 변수 선언    
  필요한 데이터를 저장하기 위한 변수부터 정의
2. 연산자 구현    
  변수를 이용하여 직접 계산하거나 처리

```python
a = 10
b = 5
result = a + b
print("결과:", result)
```

### 프로그램 코딩방식 2(함수 중심 구조적 코딩)
1. 함수 선언    
  작업을 블록 단위로 나누어 구조화(`def 함수이름():`)
2. 변수 선언    
  각 함수 내부 또는 전역에서 사용할 변수 정의
3. 연산식 구현    
  각 함수 안에서 필요한 연산 구현

```python
sum = 0; # 변수 선언 및 초기화

# 함수 선언 및 정의
def add(v1, v2):
  # 결과값을 담을 변수 선언
  result = v1 + v2;
  return result;

sum = add(300, 500);
# 800
```

## 지역 변수(Local Variable)
함수 내부에서 선언된 변수로, 해당 함수 내에서만 사용 가능  

### 특징
* **접근 범위**   
  해당 함수 **내부에서만 접근 가능**  
  함수 밖에서는 사용 불가
* **메모리상의 존재 기간**  
  **함수**가 **호출될 때 생성**되고,  
  **함수**가 **종료되면 소멸**함(자동으로 메모리에서 삭제)

## 전역 변수(Global Variable)
함수 외부에서 선언된 변수로, 모든 합수에서 접근 가능하다(단, 함수 내부에서 값을 변경하려면 `global` 키워드 필요)

### 특징
* **접근 범위**      
  **모든 함수 내에서 접근 가능**
  단, 함수 내부에서 수정하려면 `global` 선언 필요
  **global 선언을 하더라도 지역변수가 우선임**
* **메모리상의 존재 기간**    
  **프로그램이 시작될 때 생성**,    
  **프로그램이 종료될 때까지 유지**

### global 키워드 사용법

```python
a = 20  # 전역 변수

def func():
    global a  # global 선언이 먼저
    a = 10    # 그 다음 값 할당

func()
print(a)  # 10

# 잘못된 사용
def wrong_func():
    a = 10      # 지역 변수로 먼저 선언
    global a    # 에러 발생!
# SyntaxError: name 'a' is assigned to before global declaration
```
### 지역 변수와 전역 변수가 같이 있을 때

```python
x = 100 # 전역 변수

def text():
  x = 50 # 지역 변수 (전역 변수와 이름만 같음)
  print(f"함수 내부: {x}")

test()                  # 함수 내부: 50
print(f"함수 외부: {x}")  # 함수 외부: 100
```
### nonlocal 키워드
중첩 함수에서 바깥 함수의 변수를 수정할 때 사용

```python
def outer():
  x = 10

  def inner():
    nonlocal x  #  바깥 함수의 변수 수정
    x = 20

  inner()
  print(x)  # 20

outer()
```

## 매개변수(Parameter&Argument)
### 형식 매개변수(Parameter)
* 함수 정의 시 사용하는 변수
* 함수가 받을 데이터의 이름

### 실질적인 매개변수(Argument, 실인수, 인자, 인수)
* 함수 호출 시 전달하는 실제 값

```python
# 형식 매개변수 (Parameter)
def greet(name, age):   # name, age가 형식 매개변수
    print(f"안녕하세요, {name}님! {age}세시군요.")

# 실인수 (Argument)
greet("김사과", 20)   # "김사과", 20이 실인수
```
### 기본 매개변수값
함수 호출 시 인수를 전달하지 않으면 기본값이 사용됨

```python
def greet(name, greeting="안녕하세요"):
  print(f"{greeting}, {name}님!")

greet("김사과")              # 안녕하세요, 김사과님!
greet("김사과", "반갑습니다")   # 반갑습니다, 김사과님!
```
### 키워드 인수
매개변수 이름을 명시하여 값을 전달

```python
def introduce(name, age, city):
  print(f"{name}님은 {age}세이고 {city}에 살고 있습니다.")

# 위치 인수
introduce("김사과", 20, "서울")

# 키워드 인수
introduce(age=20, name="김사과", city="서울")

# 혼합 사용 (위치 인수가 먼저)
introduce("김사과", city="서울", age=20)
```
### 가변 인자
#### 가변 위치 인자(*args)
개수가 정해지지 않은 위치 인수를 받을 때 사용

```python
def sum_all(*numbers):
  total = 0
  for num in numbers:
    total += num
  return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(1, 2, 3, 4, 5))  # 15
```
#### 가변 키워드 인자(**kwargs)
개수가 정해지지 않은 키워드 인수를 받을 때 사용

```python
def print_info(**info):
  for key, value in info.items():
    print(f"{key}: {value}")

print_info(name="김사과", age=20, city="서울")
# name: 김사과
# age: 20
# city: 서울
```
#### 혼합 사용

```python
def mixed_function(a, b, *args, **kwargs):
  print(f"일반 매개변수: {a}, {b}")
  print(f"가변 위치 인자: {args}")
  print(f"가변 키워드 인자: {kwargs}")

mixed_function(1, 2, 3, 4, 5, name="김사과", age=20)
# 일반 매개변수: 1, 2
# 가변 위치 인자: (3, 4, 5)
# 가변 키워드 인자: {'name': '김사과', 'age': 20}
```

# 람다 함수(Lambda Function)
* 이름 없는 익명 함수로, 간단한 함수를 한 줄로 표현
* 일회성으로 사용될 때 유용하다.
* 때문에 불필요한 메모리 낭비를 막아준다.

```python
lambda 매개변수: 표현식
```
### 일반 함수와 람다 함수 비교

```python
# 일반 함수
def add(x, y):
  return x + y

# 람다 함수
add_lambda = lambda x, y: x + y

print(add(3, 5))        # 8
print(add_lambda(3, 5)) # 8
```
### 람다 함수 활용

```python
# map과 함께 사용
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared) # [1, 4, 9, 16, 25]

# filter와 함께 사용
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers) # [2, 4]

# sorted와 함께 사용
students = [
  {'name': '김사과', 'age': 20},
  {'name': '이바나나', 'age': 18},
  {'name': '박포도', 'age': 22}
]

sorted_students = sorted(students, key=lambda x: x['age'])
print(sorted_students)
# [{'name': '이바나나', 'age': 18}, {'name': '김사과', 'age': 20}, {'name': '박포도', 'age': 22}]
```

