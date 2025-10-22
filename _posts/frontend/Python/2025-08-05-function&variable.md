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

# 함수란?
**연산(데이터를 받아서 가공)**을 하기 위한 기능을 이야기 한다.    
![https://thebook.io/img/080398/039.jpg](함수)

## 함수의 종류(4가지)
1. 반환값을 가지는 함수
2. 반환값을 가지지 않는 함수
3. 매개변수(외부데이터)를 가지는 함수
4. 매개변수를 가지지 않는 함수

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

* **접근 범위**   
  해당 함수 **내부에서만 접근 가능**  
  함수 밖에서는 사용 불가
* **메모리상의 존재 기간**  
  **함수**가 **호출될 때 생성**되고,  
  **함수**가 **종료되면 소멸**함(자동으로 메모리에서 삭제)

## 전역 변수(Global Variable)
함수 외부에서 선언된 변수로, 모든 합수에서 접근 가능하다(단, 함수 내부에서 값을 변경하려면 `global` 키워드 필요)

* **접근 범위**      
  **모든 함수 내에서 접근 가능**
  단, 함수 내부에서 수정하려면 `global` 선언 필요
  **global 선언을 하더라도 지역변수가 우선임**
* **메모리상의 존재 기간**    
  **프로그램이 시작될 때 생성**,    
  **프로그램이 종료될 때까지 유지**

```python
a = 20

def func():
  a = 10
  global a
# name 'a' is assigned to before global declaration

def func():
  global a
  a = 10

func()
# 10
```

## 매개변수
실질적인 매개변수(인자, 인수)
형식적인 매개변수

## 활용 예시
함수의 이름은 calc()  
두 정수와 하나의 연산자를 입력하여 그 결과값을 출력하는 코드를 작성 

출력 예]  
원하는 연산자를 입력(+ - */) : *   
첫 번째 수를 입력 : 3   
두 번째 수를 입력 : 2   
계산은 3 * 2 = 6    
```python
operator = input("원하는 연산자를 입력(+ - */) : ")
num1 = int(input("첫 번째 수를 입력 : "))
num2 = int(input("두 번째 수를 입력 : "))
result = 0;

def calc(operator, num1, num2):
    if(operator == "+"):
        return num1 + num2;
    elif(operator == "-"):
        return num1 - num2;
    elif(operator == "*"):
        return num1 * num2;
    elif(operator == "/"):
        return num1 / num2;
    
result = calc(operator, num1, num2);
print("계산은 %d %s %d = %d" % (num1, operator, num2, result));

"""
원하는 연산자를 입력(+ - */) : +
첫 번째 수를 입력 : 1
두 번째 수를 입력 : 3
계산은 1 + 3 = 4
"""
```

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
```txt
!num(X) _num(O)
1_num(X) num_1(O)
```

### 변수 선언 예시
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
* 캐싱 O
  * **같은 메모리 주소 공유** 
  * **작은 정수(-5 ~ 256)**: Python이 미리 캐싱
  * **짧은 문자열**: 자동으로 intern되어 캐싱
* 캐싱 X
  * **각각 새로운 객체 생성**
  * **큰 정수(257이상)**: 매번 새 객체 생성
  * **가변 객체 (list, dict)**: 절대 캐싱 안 됨

#### 이런 차이가 생기는 이유
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

## 자료형 변환하기
* 파이썬에서는 서로 다른 자료형 간에 변환할 수 있다.
* 이를 형 변환(Type Casting)이라고 한다.

### 정수형 > 실수형
```python
a = 10
b = float(a) # 10.0
```
### 실수형 > 정수형
```python
c = 3.14
d = int(c) # 3
```
### 문자형 > 정수형, 실수형
```python
e = "100"
f = int(e) # 100
g = float(e) # 100.0
```
### 정수형/실수형 > 문자형
```python
h = 42
i = str(h) # 42

j = 3.14
k = str(j) # 3.14
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
isLover = False
print(isLover) # False
del isLover
print(isLover) # NameError: name 'isLover' is not defined
```

### 불변 객체(Immutable Object)
* 한 번 생성되면 값을 변경할 수 없는 객체이다.
* 변경하려고 하면 새로운 객체가 생성된다.

### 불변 객체 예시
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
