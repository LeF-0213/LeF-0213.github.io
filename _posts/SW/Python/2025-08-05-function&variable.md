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

## 변수 이름 규칙
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

## 변수 선언 및 초기화
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

## 변수의 범위(scope)
* 파이썬에서 변수의 범위(scope)는 해당 변수가 프로그램 내에서 참조되고 변경될 수 있는 영역을 의미한다. 
* 파이썬의 변수 범위는 크게 네 가지로 분류된다

### 지역 변수(Local Variable)
함수 내부에서 선언된 변수로, 해당 함수 내에서만 사용 가능  

#### 특징
* **접근 범위**   
  해당 함수 **내부에서만 접근 가능**  
  함수 밖에서는 사용 불가
* **메모리상의 존재 기간**  
  **함수**가 **호출될 때 생성**되고,  
  **함수**가 **종료되면 소멸**함(자동으로 메모리에서 삭제)

### 전역 변수(Global Variable)
함수 외부에서 선언된 변수로, 모든 합수에서 접근 가능하다(단, 함수 내부에서 값을 변경하려면 `global` 키워드 필요)

```python
def outer_function():
    enclosing_var = "둘러싼 범위 변수" # 2번
    
    def inner_function():
        print(enclosing_var) # 4번
    
    inner_function() # 3번

outer_function()  # 1번
```
#### 특징
* **접근 범위**      
  **모든 함수 내에서 접근 가능**
  단, 함수 내부에서 수정하려면 `global` 선언 필요
  **global 선언을 하더라도 지역변수가 우선임**
* **메모리상의 존재 기간**    
  **프로그램이 시작될 때 생성**,    
  **프로그램이 종료될 때까지 유지**

#### global 키워드 사용법

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
#### 지역 변수와 전역 변수가 같이 있을 때

```python
x = 100 # 전역 변수

def text():
  x = 50 # 지역 변수 (전역 변수와 이름만 같음)
  print(f"함수 내부: {x}")

test()                  # 함수 내부: 50
print(f"함수 외부: {x}")  # 함수 외부: 100
```
#### nonlocal 키워드
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

### 내장 범위(Built-in)
* 파이썬이 기본적으로 제공하는 내장 함수나 예외들이 있는 범위로, 
* 예를 들어 print(), len() 등이 해당된다.

```python
len = 5  # 내장 함수 'len'을 덮어씀
print(len([1, 2, 3]))  # 오류 발생! len이 함수가 아니라 변수로 인식됨
```
## 변수의 범위 탐색 순서
1. Local scope
2. Enclosing scope
3. Global scope
4. Built-in scope

> 따라서 지역 범위에 동일한 이름의 변수가 없으면 파이썬은 둘러싼 범위를 확인하고, 그다음으로 전역 범위, 마지막으로 내장 범위를 확인한다. 이렇게 변수의 범위를 이해하고 관리하는 것은 코드의 가독성과 유지 보수성을 높이고, 예기치 않은 오류를 방지하기 위함이다.

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

## None
### None의 특징
* None은 파이썬에서 특별한 값으로, 아무런 값이 없음을 표현하는 데 사용된다. 
* 다른 언어에서의 null 또는 nil과 유사한 개념이다. 
* None은 파이썬의 내장 상수이며, 그 자체로 데이터 타입이 NoneType이다. 
* 모든 None은 동일하므로, 두 개의 None 값을 비교할 때 항상 True를 반환한다.

#### 변수를 초기화 할 때 
아무런 값이 할당되지 않았음을 나타내기 위해 `None`을 사용

```python
variable = None
```
#### 아무런 값도 반환하지 않아야 할 때
* 함수에서 특정 조건에서 아무런 값도 반환하지 않아야 할 때 None을 사용한다.
* 함수에서 return 문이 생략되거나 없으면 기본적으로 None을 반환한다.

```python
def my_function(x):
    if x > 10:
        return x
# x가 10 이하일 때는 아무런 값도 반환하지 않습니다. 실제로는 None이 반환됩니다.
```
#### 함수의 매개변수에 기본값으로 None을 할당하게 할 때
* 함수의 매개변수에 기본 값으로 None을 할당하여 선택적으로 인자를 전달받을 수 있게 만들 수 있다.
* None을 검사할 때는 `==`대신 `is 연산자`를 사용하는 것이 좋다.
* `is`는 **객체의 동일성**을 검사하는 반면, `==`는 **객체의 동등성**을 검사한다.

```python
def hello(message=None):
    if message is None:
        print("Hello!")
    else:
        print(message)
```
#### 값의 존재 여부 확인
None을 사용하여 값의 존재 여부를 확인할 수 있다.

```python
def get_data_from_database():
    pass

data = get_data_from_database()
print(data) # None
if data is None:
    print('데이터를 수신하지 못함!')
else:
    print('데이터를 수신받음!')
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
### 기본값이 설정된 매개변수
함수 호출 시 인수를 전달하지 않으면 기본값이 사용됨

```python
def func(num1 = 0, num2 = 0):
  sum = num1 + num2
  return sum

print(func())           # 0
print(func(10))         # 10
print(func(10, 3))      # 13
# print(func(, 3))      # SyntaxError: invalid syntax
# print(func(None, 3))  # TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
print(func(num2 = 3))   # 3
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
### 가변 매개변수
#### 가변 위치 인자(*args)
* 개수가 정해지지 않은 위치 인수를 받을 때 사용
* 함수를 호출할 때 `*`를 사용하면 **시퀀스(리스트, 튜플 등)의 요소**를 **개별적인 위치 인자**로 풀어서 전달할 수 있다.

**기본 예시**

```python
def func(*args):
  return args

print(func7())
print(func7(10))
print(func7(10, 30, 50))
```
**활용 예시**

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
* 개수가 정해지지 않은 키워드 인수를 받을 때 사용
* 키워드 매개변수는 일반적으로 기본값이 설정된 매개변수와 함께 사용된다. 
* 함수의 매개변수에 기본값을 설정하면, 함수를 호출할 때 해당 매개변수를 생략할 수 있다.

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

# 함수란?
**데이터를 입력받아 처리하고 결과를 반환하는 기능**을 이야기한다.    
<img src="https://thebook.io/img/080398/039.jpg" width="350">

## 사용자 정의 함수
* 사용자 정의 함수란 사용자가 특정 작업을 수행하기 위해 직접 작성한 함수를 의미한다.
* 파이썬에는 많은 내장 함수들이 있지만, 
* 때로는 우리의 요구사항에 맞게 동작하는 함수를 직접 만들어야 할 때가 있다. 
* 이때 사용자 정의 함수를 작성하게 된다.

```python
def 함수명(매개변수1, 매개변수2, ...):
    # 함수 내용
    return 결과값
```

## 함수의 종류
함수는 **매개변수**와 **반환값**의 유무에 따라 4가지 형태로 나뉜다.

### 1. 매개변수 O, 반환값 O

```python
def add(a, b):
  return a + b

result = add(3, 5)
print(result) # 8

temp = add()
print(temp) # 8
```
### 2. 매개변수 O, 반환값 X

```python
def print_sum(start, end):
  sum = 0
  for i in range(start, end + 1):
    sum += i
  print(f'{start}부터 {end}까지의 합: {sum}')

print_sum(1, 10) # 1부터 10까지의 합: 55
```
### 3. 매개변수 X, 반환값 O

```python
def get_pi():
    return 3.14

pi = get_pi()
print(pi)  # 3.14

temp = get_pi()
print(temp) # 3.14
```
### 4. 매개변수 X, 반환값 X

```python
def greet():
  print("Hello, World!")

grett() # Hello, World!

temp = greet()
print(temp) # None

temp = greet()
print(temp) # <function add at 0x793ff9530cc0>
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

## 콜백 함수(Callback Function)
* 콜백 함수란 **다른 함수에 인수로 전달되어 특정 작업이 끝난 후 호출되는 함수**를 의미한다. 
* 어떤 작업이 완료된 후에 자동으로 호출되도록 미리 준비해둔 함수이다. 
* 따라서 직접적으로 호출하지 않고 **특정 조건이나 이벤트가 발생할 때** 실행되도록 설정합니다.

```python
def add(a, b):
  return a + b

def subtract(a, b):
  return a - b

def multiply(a, b):
  return a * b

def divide(a, b):
  return a // b

def calculate(x, y, operation):
  result = operation(x, y)
  print(f'결과: {result}')
  return result

calculate(10, 5, add)       # 결과: 15
calculate(10, 5, subtract)  # 결과: 5
calculate(10, 5, multiply)  # 결과: 50
calculate(10, 5, divide)    # 결과: 2
```

| 용어 | 의미 | 예시 |
|:------:|------|------|
| **콜백 함수** | 다른 함수에 **인자로 전달되는** 함수 | `add`, `subtract`, `multiply`, `divide` |
| **상위 함수** | 콜백 함수를 **인자로 받아서 호출하는** 함수 | `calculate` |
## 람다 함수(Lambda Function)
* 이름 없는 익명 함수로, 간단한 함수를 생성하기 위한 특별한 구문이다.
* "익명의 함수"라는 것은 고유한 이름이 지정되지 않았음을 의미한다.
* 람다 함수는 일반적인 함수(def를 사용하여 정의)와는 달리, 한 줄로 표현되는 짧고 간결한 함수를 생성할 때 주로 사용된다.

```python
lambda 매개변수: 표현식
```
### 람다 함수의 특징
* **간결성**: 람다 함수는 간단한 연산이나 작은 기능을 가진 함수를 간결하게 표현하는 데 유용하다.
* **익명성**: 람다 함수에는 명시적인 이름이 부여되지 않습니다. 그러나 필요에 따라 변수에 할당할 수 있다.
* **일회성**: 일반적으로 특정 작업을 위한 일회성 함수로 사용된다.

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
#### sorted()

```python
people = [
    {'name':'오렌지', 'age':30},
    {'name':'김사과', 'age':20},
    {'name':'반하나', 'age':25}
]

sorted_people = sorted(people, key=lambda x: x['age'])
print(sorted_people) # [{'name': '김사과', 'age': 20}, {'name': '반하나', 'age': 25}, {'name': '오렌지', 'age': 30}]
```
#### filter()
* filter()는 파이썬의 내장 함수로, 주어진 함수의 조건을 만족하는 항목만으로 이루어진 이터레이터를 반환한다. 
* 이 함수는 주로 리스트나 다른 순차적인 데이터 타입에서 특정 조건을 만족하는 항목들만을 필터링할 때 사용된다.

```python
li = [2, 5, 7, 10, 15, 17, 20, 22, 25, 28]

result = list(filter(lambda x: x % 2 == 0, li))
print(result) # [2, 10, 20, 22, 28]
```
#### map()
* map()는 파이썬의 내장 함수로, 주어진 함수를 이터러블의 모든 항목에 적용하여 결과를 반환하는 이터레이터를 생성한다. 
* 이 함수는 주로 리스트나 다른 순차적인 데이터 타입의 항목 각각에 함수를 적용할 때 사용된다.

```python
map(function, iterable)
```
**예시 1**

```python
num = [1, 2, 3, 4, 5]

squared_num = list(map(lambda x: x ** 2, num))
print(squared_num) # [1, 4, 9, 16, 25]
```
**예시 2**

```python
li1 = [1, 2, 3]
li2 = [4, 5, 6]

sum = list(map(lambda x, y : x + y, li1, li2))
print(sum) # [5, 7, 9]
```
**예시 3**

```python
words = ['apple', 'banana', 'orange', 'cherry']
upper_words = list(map(lambda x : x.upper(), words))
print(upper_words) # ['APPLE', 'BANANA', 'ORANGE', 'CHERRY']
```
> map() 함수는 결과를 이터레이터 형태로 반환하기 때문에 그 결과를 직접 출력하거나, 인덱싱하거나, 슬라이싱하기 위해서는 list()나 비슷한 함수를 사용하여 이터레이터를 실제 리스트나 다른 데이터 구조로 변환해야 한다.

# 함수 객체와 메모리 구조
* Python에서 함수도 **객체(First-class object)**이다.
* 함수는 변수에 할당되고, 다른 함수의 인자로 전달되며, 반환값으로 사용될 수 있다.

### 예제 1: 반환값이 있는 함수

```python
def add(a, b):
    return a + b

result = add(3, 5)
print(result)  # 8
```
![returnFunction](https://github.com/user-attachments/assets/b08b5f6f-5f5b-4bff-bcbc-fe41ec6fd290)
#### 실행 흐름
1. **함수 정의**: `add` 함수 객체가 `Heap`에 생성되고, `Code Area`에 코드 저장
2. **함수 호출**: `add(3, 5)` 호출 시 Stack에 새로운 프레임 생성
3. **매개변수 바인딩**: `a`는 `3`을, `b`는 `5`를 참조
4. **연산 실행**: `3 + 5`를 계산하여 새로운 int 객체 `8`을 생성
5. **반환**: `8` 객체의 주소를 반환하고 함수 프레임 소멸
6. **결과 저장**: `result` 변수가 반환된 `8` 객체를 참조

### 예제 2: 반환값이 없는 함수

```python
def greet():
    print("Hello!")

temp = greet()
print(temp)  # None
```
![returnX](https://github.com/user-attachments/assets/f7b5f214-8129-4b57-9dbf-5dba5a7dd8ce)

```m
**Tip**:
* Python에서 명시적인 `return`이 없으면 자동으로 `None`을 반환
* `None`은 Python 전체에서 단 하나만 존재하는 싱글톤 객체
```

### 예제 3: 함수를 변수에 할당

```python
def add(a, b):
    return a + b

temp = add  # 함수 호출 X, 함수 객체 자체를 할당
print(temp)  # <function add at 0x793ff9530cc0>
print(temp(3, 5))  # 8
```
![fuctionadd](https://github.com/user-attachments/assets/2fcf94ba-2448-4166-bc1e-1ec1bbfe9bae)

```m
**차이점 이해하기**
* `temp = add()`: 함수를 **호출**하고 반환값을 저장
* `temp = add`: 함수 객체 자체를 **참조**
```

## 핵심 정리 - 메모리 영역별 역할
### Code Area(코드 영역)
* 함수의 실제 코드(바이트코드) 저장
* 프로그램 실행 중 변경되지 않음
* 모든 함수 호출이 같은 코드를 공유

### Stack(스택)
* 변수명과 참조(메모리 주소) 저장
* 함수 호출 시 프레임(Frame) 생성
* 함수 종료 시 프레임 자동 소멸(LIFO)
* 지역 변수의 생명주기 관리

### Heap(힙)
* 실제 데이터 객체 저장
* 함수 객체, 정수, 문자열, 리스트 등 모든 객체
* 가비지 컬렉션으로 메모리 관리
* 참조 카운트가 0이 되면 삭제

## 메모리 제거
* 함수도 파이썬 객체이므로 참조 카운팅과 가비지 컬렉션의 원칙에 따라 동작한다. 
* 함수를 참조하는 변수나 요소가 없게 되면 해당 함수는 가비지 컬렉터에 의해 메모리에서 제거될 수 있다. 
* del 명령어를 사용하여 함수에 대한 참조를 명시적으로 제거할 수 있다. 
* 하지만 이것이 함수가 즉시 메모리에서 제거된다는 것을 보장하지는 않는다.

```python
def func():
  print("함수 출력")

func() # 함수 출력

del func
func() # NameError: name 'func' is not defined
```