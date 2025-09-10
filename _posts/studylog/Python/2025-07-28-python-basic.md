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
* **풍부한 표준라이브러리를 제공하는 다목적 언어**
파이썬은 파일 처리, 수학, 문자열, 네트워크, 데이터베이스, 웹 프로그래밍 등 다양한 기능을 내장 라이브러리로 지원하여, 별도의 복잡한 설정 없이도 손쉽게 활용할 수 있다.
* **절차적 언어 + 객체지향 언어**
함수 단위로 코드를 작성하는 절차적 프로그래밍과, 클래스를 활용한 객체지향 프로그래밍을 모두 지원한다. 따라서 간단한 스크립트 작성부터 대규모 애플리케이션 개발까지 유연하게 적용할 수 있다.
* **인터프리터 언어**
별도의 컴파일 과정 없이, 작성한 코드를 한 줄씩 해석하며 실행하는 방식이다. 덕분에 실행 결과를 빠르게 확인할 수 있고, 코드 수정과 테스트 과정이 간편하다. 다만, 대규모 연산에서는 컴파일 언어(C, Java 등)에 비해 속도가 느릴 수 있다.

### 자동형변환(동적타이핑)
* 파이썬은 변수 선언 시 자료형을 지정하지 않아도 되고,
* 실행 중에 자동으로 형이 결정된다.
```python
  a = 10;
  a = 10.5;
  a = 'A'
```

### 인터프리터 언어
* 컴파일 과정 없이 `.py`파일을 직접 실행한다
* 장점: 실행이 간단하다
* 단점: 실행 속도가 느리다

### 함수란?
* 모든 프로그램은 함수의 집합이다.
* 함수는 특정 **기능을 수행하는 코드 블록**이다.
```python
print("Hello Phthon")
```

# 변수란?
* 변수를 선언하면 반드시 **초기화** 후 사용해야 한다.
* **단일 데이터**를 담는 이름표 역할
### 변수 이름 규칙
변수 이름은 문자, 숫자, 언더바(_)만 사용 가능
```txt
!num(X) _num(O)
1_num(X) num_1(O)
```
### 변수 선언 예시
파이썬에서는 변수 선언 시 자료형을 따로 명시하지 않아도 된다.
하지만 사용하기 전에 초기화하는 것이 중요하다.
* **정수형(int)** → 0
* **실수형(float)** → 0.0
* **문자형(str, 한 글자 문자열)** → '' (빈 문자열)
* **문자열(str)** → "" 또는 None
```python
number = 0        # 정수 초기화
pi = 0.0          # 실수 초기화
letter = ''       # 문자 초기화 (파이썬은 char 자료형이 없고 str 사용)
text = None       # 문자열 초기화 (아직 값 없음)
```
## 변수와 메모리
파이썬은 **리터럴 캐싱**으로 동일한 불변 객체를 재사용
### 리터럴 공유(객체 캐싱)
리터럴 공유는 동일한 값을 가진 불변 객체가 여러 곳에서 사용될 때, 새로운 객체를 만들지 않고 기존 객체를 재사용하는 방식이다. 이를 통해 파이썬은 메모리 사용을 최적화할 수 있습니다.
```python
a = 200
b = 200
print(a)
print(b)
```
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbnojEK%2FbtsJ9njyXf2%2FAAAAAAAAAAAAAAAAAAAAAADUZKeupRfBbZWCOOT_JVOURYE2KlaiCeXC3M4srYSy%2Fimg.webp%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DC6VexxEJr8YlEAXNvXYIXh009qo%253D)

# 츌력 형식
## f-string
```python
name = "banana"
age = 25
height = 160.123
print(f"이름: {name}, 나이: {age}, 키: {height:.1f}")
```
## 출력서식지정자(%)
파이썬은 변수를 선언할 때 자료형을 명시하지 않아도 되지만,
출력할 때 `%`연산자를 이용하면 **자료형에 맞는 출력 형식**을 지정할 수 있다.
### 주요 서식 지정자
| 서식 지정자 | 설명              | 예시                                  |
| ------ | --------------- | ----------------------------------- |
| `%d`   | 10진수 정수         | `print("%d" % 100)` → `100`         |
| `%x`   | 16진수 정수         | `print("%x" % 255)` → `ff`          |
| `%o`   | 8진수 정수          | `print("%o" % 8)` → `10`            |
| `%f`   | 실수 (기본 소수점 6자리) | `print("%f" % 3.14)` → `3.140000`   |
| `%.nf` | 소수점 n자리까지 출력    | `print("%.2f" % 3.14159)` → `3.14`  |
| `%c`   | 문자 (유니코드 값 가능)  | `print("%c" % 65)` → `A`            |
| `%s`   | 문자열             | `print("%s" % "Python")` → `Python` |
### 출력서식지정자 예시
1. 정수 출력
```python
print("%5d" % 123)   # 전체 5칸 확보, 오른쪽 정렬 → "  123"
print("%05d" % 123)  # 전체 5칸 확보, 왼쪽 빈칸을 0으로 채움 → "00123"
```
2. 실수 출력
`%m.nf`에서
`m` → 전체 출력 칸 수 (소수점 포함)
`n` → 소수점 자리수
```python
print("%7.3f" % 123.45)  # 전체 7칸 확보, 소수점 3자리 → "123.450"
print("%07.2f" % 123.45) # 전체 7칸 확보, 소수점 2자리, 왼쪽 빈칸 0으로 채움 → "0123.45"
```
3. 주의 사항
전체 칸(`m`)이 실제 출력 폭보다 작으면 무시된다.
```python
print("%3.3f" % 123.45)  # 전체 3칸 지정은 무시되고 소수점 3자리만 적용 → "123.450"
```

# 연산자
## 산술연산자
| 연산자 | 의미 | 예시 (a = 10, b = 3) | 결과 |
| ---- | --- | ------------------- | --- |
| `+` | 덧셈 | `a + b` | 13 |
| `-` | 뺄셈 | `a - b` | 7 |
| `*` | 곱셈 | `a * b` | 30 |
| `/` | 나눗셈 (실수) | `a / b` | 3.333... |
| `//` | 몫 (정수 나눗셈) | `a // b` | 3 |
| `%` | 나머지 | `a % b` | 1 |
| `**` | 거듭제곱 | `a ** b` | 1000 |
### 활용 예시 - 동전 교환 프로그램
```python
"""
동전 교환 프로그램을 작성하시오
사용자가 입력한 금액을 500원, 100원, 50원, 10원으로 교환하고자 합니다.
교환 방법은 가장 큰 동전부터 교환을 한다.

사용자가 금액을 => 2430
500원 => 4개
100원 => 4개
10원 => 3개
"""
price = 2430;
coin_500 = price / 500; price %= 500;
coin_100 = price / 100; price %= 100;
coin_50 = price / 50; price %= 50;
coin_10 = price / 10;

print("500원 => %d개" % coin_500);
print("100원 => %d개" % coin_100);
print("50원 => %d개" % coin_50);
print("10원 => %d개" % coin_10);
```
### 관계연산자
| 연산자  | 의미    | 예시       | 결과    |
| ---- | ----- | -------- | ----- |
| `==` | 같다    | `5 == 3` | False |
| `!=` | 같지 않다 | `5 != 3` | True  |
| `>`  | 크다    | `5 > 3`  | True  |
| `<`  | 작다    | `5 < 3`  | False |
### 논리연산자
| 연산자   | 의미     | 예시 (x=True, y=False) | 결과    |
| ----- | ------ | -------------------- | ----- |
| `and` | 논리 AND | `x and y`            | False |
| `or`  | 논리 OR  | `x or y`             | True  |
| `not` | 논리 부정  | `not x`              | False |
```python
# and
print(True and 3)       # 3
print(3 and 5)         # 5
print(0 and 5)         # 0

# or
print(False or 3)      # 3
print(3 or 5)          # 3
print(0 or 5)          # 5

# not
print(not True)        # False
print(not 0)           # True
print(not 3)           # False
```
#### Boolean 평가 규칙
**True로 평가되는 값**
* 0이 아닌 모든 숫자
* 비어있지 않은 문자열/리스트/튜플/세트/딕셔너리
* True
**False로 평가되는 값**
* False
* 0
* 빈 문자열 ""
* 빈 컨테이너 ([], (), {}, set())
* None

### 복합연산자
복합연산자는 **산술, 비교, 논리 연산**을 한 줄에서 간단히 표현할 때 사용한다.
```python
num = 10

# 논리 AND
print((num > 5) and (num < 15))  # True, 두 조건 모두 True여야 True
# 논리 OR
print((num > 5) or (num < 5))    # True, 둘 중 하나만 True여도 True
# 논리 NOT
print(not(num < 20))             # False, 조건을 부정
```
### 비트연산자
비트연산자는 **정수를 2진수로 변환하여 비트 단위로 연산**한다.
| 연산자  | 의미      | 예시            |     |         |
| ---- | ------- | ------------- | --- | ------- |
| `&`  | AND     | `5 & 3` → 1   |     |         |
| \`   | \`      | OR            | \`5 | 3\` → 7 |
| `^`  | XOR     | `5 ^ 3` → 6   |     |         |
| `~`  | NOT     | `~5` → -6     |     |         |
| `<<` | 왼쪽 시프트  | `5 << 1` → 10 |     |         |
| `>>` | 오른쪽 시프트 | `5 >> 1` → 2  |     |         |
```python
a = 60  # 111100
b = 13  # 001101

print(a & b)  # 12, 즉 1100
print(a | b)  # 61, 즉 111101
print(a ^ b)  # 49, 즉 110001
print(~a)  # -61
print(a << 2)  # 240, 즉 11110000
print(a >> 2)  # 15, 즉 1111
```

## 주석(Comments)
주석 유형 | 문법 | 용도
---------|------|------
**한 줄 주석** | `# 주석 내용` | 간단한 설명이나 임시 코드 비활성화
**여러 줄 주석** | `""" 주석 내용 """` | 여러 줄에 걸친 설명이나 코드 블록 비활성화
**문서화 주석** | `/** 주석 내용 */` | JavaDoc 도구로 API 문서 자동 생성용

# 조건문
실행하기 위해서는 실행할 수 있는 구간이 필요 `{}`  
파이썬은 `{}`아닌 `:`을 사용하여 블럭문을 표시한다
## if문
```python
if num > 100:
    print("참입니다.")
else:
    print("거짓입니다.")
```
## 중첩if문
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
## Walrus Operator(x := expression)
`expression`을 계산한 값을 `x`에 할당하고, 동시에 그 값을 사용
```python
text = input("아이디를 입력하세요: ")

if (length := len(text)) < 3:
    print(f"아이디가 너무 짧습니다. (입력한 글자 수: {length})")
else:
    print(f"아이디 '{text}'는 사용 가능합니다.")
```
## 조건부 표현식
```python
# if~else로 작성
num = int(input('숫자를 입력하세요: '))
if num % 2 == 0:
    print('짝수')
else:
    print('홀수')

# 조건부 표현식으로 작성
num = int(input('숫자를 입력하세요: '))
print('짝수') if num % 2 == 0 else print('홀수')
```
## 구조적 패턴 매칭(`match - case`)
```python
match 값:
    case 패턴1:
        실행할 코드1
    case 패턴2:
        실행할 코드2
    case _:
        기본 실행 코드 (default)
```
### 얘시 1 - 월별 일수 확인
```python
month = int(input("월을 입력하세요 (1~12): "))

match month:
    case 1 | 3 | 5 | 7 | 8 | 10 | 12:
        print(f"{month}월은 31일까지 있습니다.")
    case 4 | 6 | 9 | 11:
        print(f"{month}월은 30일까지 있습니다.")
    case 2:
        print("2월은 28일 또는 윤년이면 29일까지 있습니다.")
    case _:
        print("잘못된 월입니다. 1~12 사이의 숫자를 입력해주세요.")
```
### 예시 2 - 사용자 연령 분류
```python
user = ("김사과", 20)

match user:
    case (name, age) if age > 19:
        print(f"{name}님은 성인입니다.")
    case (name, age) if age > 15:
        print(f"{name}님은 청소년입니다.")
    case (name, age) if age > 6:
        print(f"{name}님은 어린이입니다.")
    case _:
        print(f'{name}님은 유아입니다.')
```
### 예시 3 - 자료구조 패턴 매칭
```python
# scores = [95, 88, 76]
# scores = (95, 88, 76, 100)
scores = {"국어":95, "영어":88, "수학":76}

match scores:
    case [korean, english, math]:
        print(f"1. 국어: {korean}, 영어: {english}, 수학: {math}")
    case (korean, _, math, computher):
        print(f"2. 국어: {korean}, 수학: {math}, 컴퓨터: {computer}")
    case {"국어": korean, "영어": english, "수학": math}:
        print(f"3. 국어: {korean}, 영어: {english}, 수학: {math}")
```

### 활용예시 - 계산기
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

# 반복문
파이썬의 `range()`는 시작값 ≤ i < 끝값 범위에서 동작한다.
range(시작값, 끝값, 증가값)
```python
for i in range(0, 3, 1):
    print("%d: 처음 해보는 파이썬" % (i + 1));
    # 1: 처음 해보는 파이썬
    # 2: 처음 해보는 파이썬
    # 3: 처음 해보는 파이썬
```
**한줄에 쓰고 싶을 때**
```python
for i in range (1, 6):
    print(i, end = " "); # 1, 2, 3, 4, 5
```

### 활용 예시 - 구구단
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

## enumerate() 함수
반복문에서 **인덱스와 값을 함께** 가져올 때 사용.
```python
enumerate(iterable, [start=0])
```
### 예시 1
```python
for e in enumerate('hello', 0):
    print(e)
"""
(0, 'h')
(1, 'e')
(2, 'l')
(3, 'l')
(4, 'o')
"""
```
### 예시 2
```python
list1 = [10, 20, 30, 40]

for i, v in enumerate(list1):
    print(f'인덱스:{i}, 값:{v}')
"""
인덱스:0, 값:10
인덱스:1, 값:20
인덱스:2, 값:30
인덱스:3, 값:40
"""
```

## Iterable & Iterator
### Iterable(이터러블)
* for문에서 **반복 가능한 객체**를 의미
* 리스트, 튜플, 문자열, 딕셔너리, 세트(set) 등이 모두 이터러블
* 특징: `iter()`를 적용할 수 있다.
* **순서가 있는 것**(list, tuple, str)도 있고, **순서 없는 것**(set, dict.keys())도 있다.
```python
numbers = [10, 20, 30]  # 리스트는 이터러블
for n in numbers:
    print(n)
```
### Iterator(이터레이터)
* 이터러블 객체를 `iter()` 함수로 변환하면 만들어지는 객체
* `next()` 함수를 사용해 값을 하나씩 꺼낼 수 있다.
* 값을 다 꺼낸 후 더 `next()` 를 호출하면 **Stoplteration 예외**가 발생한다. 
```python
numbers = [10, 20, 30]

# 이터러블 → 이터레이터로 변환
iterator = iter(numbers)

print(next(iterator))  # 10
print(next(iterator))  # 20
print(next(iterator))  # 30
# print(next(iterator))  # StopIteration 발생
```

## zip() 함수
여러 시퀀스를 묶어서 반복 처리 가능.
### 예시 1(기본 range() 함수)
```python
li1 = [10, 20, 30]
li2 = ['apple', 'banana', 'orange']

for i in range(len(li1)):
    print((li1[i], li2[i]))
"""
(10, 'apple')
(20, 'banana')
(30, 'orange')
"""
```
### 예시 2
```python
li1 = [10, 20, 30, 40, 50]
li2 = ['apple', 'banana', 'orange']

for l1, l2 in zip(li1, li2):
    print(l1, l2)
"""
10 apple
20 banana
30 orange
"""
```
### 예시 3(튜플 반환)
```python
li1 = [10, 20, 30]
li2 = ['apple', 'banana', 'orange']

for li in zip(li1, li2):
    print(li)
"""
(10, 'apple')
(20, 'banana')
(30, 'orange')
"""
```

## 알아두면 좋은 게냠
### 부동소수점 오차
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FdIvD5n%2FbtsKpllrAUz%2FAAAAAAAAAAAAAAAAAAAAAPyW2WWUgkEBN3AMLucwS24KXgcBV79-0_GFRUjKYhhC%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DtD03NE3kMe%252BIAXe8AFhe9kgmlwY%253D)
```python
print(1.1 + 0.1 == 1.2)  # False
```
* 컴퓨터는 10진수를 2진수로 변환하여 계산한다.
* 하지만 `0.1`, `1.1` 같은 값은 2진수로 무한 소수가 되기 때문에, 메모리에 근사값으로 저장된다.
* 따라서 `1.1 + 0.1`은 `1.2`와 거의 같지만, 정확히 같지 않아서 `False`가 나온다.

### sys.float_info.epsilon
* 파이썬의 `float`는 **64비트 배정밀도(Double precision, IEEE 754)** 를 사용한다.
* `epsilon`은 **1과 1.0 사이에서 구분 가능한 가장 작은 차이값**이다.
* 즉, 이 값보다 작은 차이는 float 연산에서 동일한 값으로 처리된다.
```python
import sys
print(sys.float_info.epsilon)
```

### 데이터의 크기를 나타내는 단위
* **Bit** : 0 또는 1을 저장할 수 있는 최소 단위  
* **Byte** : 8bit, 문자 하나를 저장할 수 있는 기본 단위  
* **KB (킬로바이트)** : 1,024 Byte  
* **MB (메가바이트)** : 1,024 KB (약 1,048,576 Byte)  
* **GB (기가바이트)** : 1,024 MB (약 1,073,741,824 Byte)  
* **TB (테라바이트)** : 1,024 GB (약 1,099,511,627,776 Byte)  
* **PB (페타바이트)** : 1,024 TB  
* **EB (엑사바이트)** : 1,024 PB  
* **ZB (제타바이트)** : 1,024 EB  
* **YB (요타바이트)** : 1,024 ZB  


