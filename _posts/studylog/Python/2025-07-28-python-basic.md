---
layout: post
related_posts:
    - /studylog/python
title:  "파이썬 기본 개념"
date:   2025-07-29
categories:
  - studylog
  - python
description: >
  파이썬의 기초 개념부터 변수, 함수, 연산자, 조건문, 반복문까지 기본 내용을 정리
---
* toc
{:toc .large-only}

# 파이썬이란?
풍부한 표준라이브러리, 

## 언어
- 절차적언어
- 객체지향언어
=> 
다목적 언어
인터프리터 언어


## 자동형변환(동적타이핑)
데이터형(type)
정수(byte, int) 실수(float, double) 문자(char) 문자열(String)
```python
  a = 10;
  a = 10.5;
  a = 'A'
```


## 변수란?
사용하기 전에 반드시 초기화 되어야 한다.
변수 = 단수데이터
1. 변수 이름은 문자, 숫자, 언더바(_)만 사용 가능
    ```txt
    !num(X) _num(O)
    1_num(X) num_1(O)
    ```
2. 

**변수 선언**  
  반드시 변수를 선언하면 사용하기 전에 초기화 해주어야 한다.  
  정수 => 0   
  실수 => 0.0   
  문자 => NULL    

## 함수란?
모든 프로그램은 함수의 집합이다.
함수 ==> 기능
```python
  print("Hello Phthon")
```

## 인터프리터 언어
**컴파일 없이 바로 실행**
- .c --> .exe
- .java --> .class
- .py --> .py

**단점**
- 실행속도가 느리다

## f-string

## 출력서식지정자
파이썬 => 데이터 타입을 선언하지 않아도 자동형변환  
유일하게 파이썬이 데이터 형을 선언하는 부분 => 출력서식지정자(%)

정수 %d print("test %d"), %x => 16진수, %o => 8진수  
실수 %f(float)   
문자 %c(char)   
문자열 $s(string)

print("%d" % 100)

실수는 => 소수점 6자리까지 출력

not enough arguments for format string
원소의 개수는 문자열에 있는 츌룍 서식의 개수와 일치해야 한다.
```python
print("%d + %d" % (100, 100))

print("%5d" % 123)
#  123
print("%05d" % 123)
#00123
```

```python
print("%7.3f" % 123.45)
#123.450
print("%07.2f" % 123.45)
#0123.45
```
소숫점 포함 7칸을 내어준다.  

다만 아래와 같은 경우는 무시하고 출력한다.
```python
print("%3.3f" % 123.45)
#123.450
```

```python
num1 = int(input())  
num2 = int(input())

result = num1 + num2
print(result)
```

`input()` 함수는 입력되는 모든 값을 문자열로 인식한다.

## 연산자
### 산술연산자
| 연산자 | 의미 | 예시 (a = 10, b = 3) | 결과 |
| ---- | --- | ------------------- | --- |
| `+` | 덧셈 | `a + b` | 13 |
| `-` | 뺄셈 | `a - b` | 7 |
| `*` | 곱셈 | `a * b` | 30 |
| `/` | 나눗셈 (실수) | `a / b` | 3.333... |
| `//` | 몫 (정수 나눗셈) | `a // b` | 3 |
| `%` | 나머지 | `a % b` | 1 |
| `**` | 거듭제곱 | `a ** b` | 1000 |

### 활용 예시
동전 교환 프로그램을 작성하시오
사용자가 입력한 금액을 500원, 100원, 50원, 10원으로 교환하고자 합니다.
교환 방법은 가장 큰 동전부터 교환을 한다.

사용자가 금액을 => 2430
500원 => 4개
100원 => 4개
10원 => 3개

```python
price = 2430;
coin_500 = price / 500;
price %= 500;
coin_100 = price / 100;
price %= 100;
coin_50 = price / 50;
price %= 50;
coin_10 = price / 10;

print("500원 => %d개" % coin_500);
print("100원 => %d개" % coin_100);
print("50원 => %d개" % coin_50);
print("10원 => %d개" % coin_10);
```

### 관계연산자
관계연산자 | 의미 | 예시 (a = 5, b = 3) | 결과
------- | --- | ------------------ | ---
**==** | 같다 | `a == b` | False
**!=** | 같지 않다 | `a != b` | True
**>** | 크다 | `a > b` | True
**<** | 작다 | `a < b` | False
**>=** | 크거나 같다 | `a >= b` | True
**<=** | 작거나 같다 | `a <= b` | False

### 논리연산자
논리연산자  | 의미 | 예시 (x = True, y = False) | 결과
----- | ----------- | ---------------------------- | ------- 
**and** | 논리 AND | `x and y` | False |
**or** | 논리 OR  | `x or y` | True |
**not** | 논리 부정  | `not x` | False |

**복합연산자**  
```python
num = 10;
(num > 5) and (num < 10)
(num > 5) or (num < 10)
not(num < 20)
```

### 비트연산자   
10 + 10 / 10 - 2 => 10 + (-2)



## 형식지정자(%[-혹은0]n.m[지정형식])
형식 | 설명 | 예시 값    
----|------------------|--------
**%d**| 10진수 정수 출력 | 10      
**%f** | 실수 출력 (기본 소수점 6자리) | 3.141593
**%s** | 문자열 출력 | "hello" 
**%c** | 문자 출력 | 'A'     
**%b** | boolean 출력 | true    
**%%** | `%` 문자 출력 | %       
**%n** | 줄바꿈 (OS에 맞게 처리됨) | -       

**사용예시**
```python
num = 42;
pi = 3.14159;
name = "Lef";

print("정수: %d" % num);           // 정수: 42
print("실수: %.2f" % pi);         // 실수: 3.14
print("문자열: %s" % name);       // 문자열: Lef
```

## 주석(Comments)
주석 유형 | 문법 | 용도
---------|------|------
**한 줄 주석** | `# 주석 내용` | 간단한 설명이나 임시 코드 비활성화
**여러 줄 주석** | `""" 주석 내용 """` | 여러 줄에 걸친 설명이나 코드 블록 비활성화
**문서화 주석** | `/** 주석 내용 */` | JavaDoc 도구로 API 문서 자동 생성용

## 조건문
실행하기 위해서는 실행할 수 있는 구간이 필요`{}`  
=> 파이썬은 `{}`아닌 `:`을 사용하여 블럭문 표시
**if문**
```python
if num > 100:
    print("참입니다.")
else:
    print("거짓입니다.")
```

**중첩if문**
```python
if num > 90:
    print("A 학점")
elif num > 80:
    print("B 학점")
elif num > 70:
    print("C 학점")
else:
    print("D 학점")
```

### 활용예시
```python
# 문제) 다음과 같은 결과가 나올 수 있게 작성하시요  
# 첫번째 정수 입력 : 5  
# 원하는 연산자를 입력(+ - * /) :   
# 두번째 정수 입력 : 2  
# 5 * 2 = 10입니다.

num1 = int(input("첫번째 정수 입력 : "));
operator = input("원하는 연산자를 입력(+ - * /) : ");
num2 = int(input("두번째 정수 입력 : "));
result = 0;

if operator == "+":
    result = num1 + num2;
elif operator == "-":
    result = num1 - num2;
elif operator == "*":
    result = num1 * num2;
elif operator == "/":
    result = num1 / num2;
else:
    print("연산자가 아닙니다.")

print("%d %s %d = %d입니다." % (num1, operator, num2, result));
```

## 반복문
```python
for i in range(0, 3, 1):
    print("%d: 처음 해보는 파이썬" % (i + 1));
    # 1: 처음 해보는 파이썬
    # 2: 처음 해보는 파이썬
    # 3: 처음 해보는 파이썬
```
range(시작값, 끝값, 증가값)
파이썬은 무조건 범위가 `시작값 <= i < 끝값`이다.

**한줄에 쓰고 싶을 때**
```python
for i in range (1, 6):
    print(i, end = " "); # 1, 2, 3, 4, 5
```

### 활용 예시
```python
start = int(input("시작값 입력 : "));
end = int(input("끝값 입력 : "));
inc = int(input("증가값 입력 : "));
sum = 0;

for i in range(start, end + 1, inc):
    sum += i;
print("%d에서 %d까지 %d씩 증가한 값의 합은 : %d" % (start, end, inc, sum));
```

구구단(이중 for문)
```python
for i in range(0, 10, 1):
    for k in range(2, 10, 1):
        if(i == 0):
            print("  < %d단 > " % k, end = "\t")
        else:
            print("%d X %d = %2d" % (k, i, (i * k)), end = "\t");
    print();
```

## 변수
파이썬에서 변수는 데이터를 저장하기 위한 이름표와 같은 역할을 합니다. 변수에 값을 할당하면, 그 값은 컴퓨터 메모리에 저장되고 변수 이름은 그 값을 가리키게 됩니다. 파이썬은 동적 타입 언어이기 때문에, 변수를 선언할 때 자료형을 명시하지 않아도 되며, 한 번 정해진 변수에 다른 자료형의 값을 다시 저장할 수도 있습니다.
```python
name = '김사과'
age = 20

print(name)
print(age)
```

### 리터럴 공유(객체 캐싱)
리터럴 공유는 동일한 값을 가진 불변 객체가 여러 곳에서 사용될 때, 새로운 객체를 만들지 않고 기존 객체를 재사용하는 방식입니다. 이를 통해 파이썬은 메모리 사용을 최적화할 수 있습니다.
```python
a = 200
b = 200
print(a)
print(b)
```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbnojEK%2FbtsJ9njyXf2%2FAAAAAAAAAAAAAAAAAAAAAADUZKeupRfBbZWCOOT_JVOURYE2KlaiCeXC3M4srYSy%2Fimg.webp%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DC6VexxEJr8YlEAXNvXYIXh009qo%253D)


0.399999999 -> 부동 소수형 데이터를 이진법
sys.float_info.epsilon

Collection
여러 값을 담는 데이터 타입을 이야기한다.
대표적인 것이 리스트, 튜플, 딕셔너리, 셋, 문자열 등이 존재
이것들이 수정 여부에 따라 변경할 수 있는 것(mutable 객체), 변경하지 못하는 것(immutable 객체)

mutable 객체 => 리스트, 딕셔너리, 셋 <- 코테에서 모든 결과값은

리스트(배열)
1. ArrayList
2. LinkedList

슬라이싱
컴프리핸션 => 반복문 조건문을 사용하여 리스트를 생성할 수 있는 문법
객체를 순환

딕셔너리(자료구조) => 해시 테이블

set => 해시테이블 기반으로 구현으로 자료구조(집합 UNION)
set 초기화 => {}

클래스 
사용자가 정의하는 새로운 데이터 타입

## 자료구조 
1. 선형 => 리스트[]. (), {}, 스택(FILO), 큐(FIFO)
2. 비선형 => 트리(힙), 그래프
+ 알고리즘

### 리스트(List)
* 파이썬에서 기본적으로 제공해주는 자료구조
* 리스트 == 배열(순차적으로 나열되어 있다)
* 두 가지로 나누어서 이야기 한다. (1. 배열 리스트 2. 연결리스트)

1. 빈 리스트 생성 (num = [])
2. 리스트에 데이터(요소, item) 삽입
insert(), put(), add(), append() => 모든 자료구조의 명령어는 ADT(추상형 데이터)
삭제 함수 pop(), remove(), clear() 현재 마지막 데이터를 삭제
pop(index) => num

### 연결 리스트
가장 배열의 공간 낭비를 피할 수 있는 자료구조이다.
데이터를 추가될 때마다 공간을 할당 받아서 사용한다(동적 할당)

Node data, item(요소) next, link(주소)
노드의 존재 의미는 데이터
노드는 데이터를 가지지 않을 수 있다 => 헤더(dummy)

