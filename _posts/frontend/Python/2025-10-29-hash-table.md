---
layout: post
related_posts:
    - /frontend/python
title:  "[자료 구조]해시 테이블(Hash Table)"
date:   2025-10-29
categories:
  - frontend
  - python
description: >
  해시 테이블의 개념과 해시 충돌이 왜 발생하는지 그 해결방안은 무엇인지에 대한 정리
---
* toc
{:toc .large-only}

# 해싱(Hashing)이란?


# 해시 테이블(Hash Table)이란?
> 해시 테이블은 **키(Key)와 값(Value)을 매핑하여 저장하는 자료구조**이다.     
> 해시 함수를 사용해 키를 배열의 인덱스로 변환하여, 평균적으로 **O(1) 시간복잡도**로 데이터를 저장하고 검색할 수 있다.      
> (배열이 인덱스 번호로 O(1)만에 해당 위치의 값을 찾아낼 수 있는 것처럼 매우 큰 숫자 데이터나 문자열과 같이 임의의 길이를 가진 Key값을 고정된 작은 값으로 매핑하는 원리)

## 기본 구조
![hashtable](https://laboputer.github.io/assets/img/algorithm/ds/07_hashtable1.PNG)

### 구성 요소
* **버킷(Bucket)**: 데이터를 저장하는 배열의 각 슬롯(열) (버킷이 집이고 슬롯이 방, 슬롯 안 데이터는 동거자 개념, 버킷 == 주소)
* **해시 코드**: 키에 대응되는 고유한 정수값
* **해시 함수**: 키를 입력받아 고정된 크기의 해시 코드로 변환하는 함수
* **인덱스**: 실제 배열에서 데이터가 저장될 위치

## 해시 테이블을 써야하는 이유
### 다른 자료구조와의 비교
> 리스트를 사용했을 경우, 최악의 경우 O(n)의 시간복잡도를 가지게 된다.      
> 모든 원소를 순차적으로 탐색해야 하기 때문에 맨 끝에 있으면 모든 노드를 거쳐가야 하기에 시간이 오래 걸린다.

### 해시 테이블의 혁신: O(1) 접근
> 그런데 만약 데이터의 위치를 바로 계산할 수 있다면?      
> -> O(1) 상수 시간이 걸리게 되는 것이다.

![hash table](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FG1cNO%2FbtrKcvnJ4Ea%2FAAAAAAAAAAAAAAAAAAAAAJcvKvDQdV1nOt9Val7lQHEBnRMcmKqQW2n-5IcNuiF5%2Fimg.jpg%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DazxpFkNNbCRIN2DJT2HRl3H6fjA%253D)

> 각 과일별 가격을 저장한다고 가정한다. 이때, 각 과일은 키(key)가 될 것이고 과일의 가격은(value)로 해시 테이블에 저장되게 될 것이다.    
> 우리가 만약 바나나를 해쉬 함수(Hash Function)에 넣는다면, 바나나의 해시 값(Hash Value) 4가 산출되어 해시 테이블의 인덱스 4번에 바나나의 가격 1800이 저장된다.   
> 즉, 우리가 나중에 바나나의 가격을 알고 싶을 때, 바로 4번 인덱스에 접근하면 된다.

**핵심**
* 해시 함수는 **단순 계산** (덧셈, 곱셈, 나머지 연산)
* 배열 인덱스 접근은 **메모리 주소 계산**만 하면 되기에
* 데이터가 1만개든 100만개든 **계산량은 동일**하다.

### 예시
**데이터 구조**

```python
# 저장할 데이터
과일(Key)    가격(Value)
--------------------------
apple    →   3000원
banana   →   1800원
pear     →   3200원
watermelon    →   7900원
```
**해시 함수 적용**

```python
# Division Method와 함께 사용하지 않기에 table_size에 소수를 사용하지 않음
def hash_function(key, table_size=10):
  # Digit Folding 방식 사용(밑에 자세한 설명 있음)
  # hash_value = sum(ord(char) for char in key)
  # return hash_value % table_size
  return sum(ord(char) for char in key) % table_size
```
**해시 테이블에 저장**

```python
# 해시 테이블 초기화 (크기 10)
hash_table = [None] * 10

# 데이터 삽입 과정
def insert(key, value):
  hash_value = hash_function(key, 10) # 단순 산술 연산 -> O(1)
  hash_table[hash_value] = value      # 배열 인덱스 접근 -> O(1) 
  print(f"'{key}' -> 해시값 {hash_value} -> 테이블[{hash_value}]에 {value}원 저장")

# 실제 삽입
insert('apple', 3000)
insert('banana', 1800)
insert('pear', 3200)
insert('watermelon', 7900)

# 결과
'apple' -> 해시값 0 -> 테이블[0]에 3000원 저장
'banana' -> 해시값 9 -> 테이블[9]에 1800원 저장
'pear' -> 해시값 4 -> 테이블[4]에 3200원 저장
'watermelon' -> 해시값 6 -> 테이블[6]에 7900원 저장
```

# 해쉬 함수(Hash Function)이란?
> 해쉬 함수란, 주어진 데이터(Key)를 고유한 숫자 값인 해시 값(Hash Value)로 표현해 주는 함수
> 모든 입력에 대해 고정된 크기(ex. K값 이하)의 데이터로 변경하는 함수   

## 해시 함수의 특징
* **일방향성**: H(x)를 통해 얻은 Y값으로 다시 X값을 얻을 수 없음
* **결정론적**: 같은 입력은 항상 같은 출력을 생성
* **충돌 가능성**: 다른 입력 X1, X2가 같은 출력 Y를 만들 수도 있음 
-> **해시 충돌(Hash Collision)** 발생

## 해쉬 함수의 종류
### Division Method(나눗셈 법)
입력값을 테이블 크기로 나눈 **나머지**를 해시 값으로 사용하는 방법    

```python
h(k) = k mod m
```   
> 가장 간단하고 빠른 방법     
> 테이블 크기 m은 **소수(prime number)**를 사용하는 것이 좋다.      
> **2의 제곱수**는 피하는 것이 좋다.(비트 패턴에 의존하기 때문)

**예시**

```python
def hash_division(key, table_size):
  return key % table_size

# table_size = 13(소수)
print(hash_division(25, 13)) # 12
print(hash_division(38, 13)) # 12 (❗️충돌 발생)
```
### Digit Folding(자릿수 접기)
* 키를 **동일한 크기의 부분(digit)으로 분할(folding)**하여 각 부분을 **더하거나 XOR 연산**하여 해시 값(결과 값 % 테이블 크기)을 생성한다.
* 문자열일 경우, 각 문자를 ASCII 코드로 변환하고, 모든 ASCII 값을 합산한 결과값을 테이블 크기로 나눈 나머지를 해시 값(주소)로 사용한다.

> **장점**: 구현이 간단하고, 키의 모든 부분을 고려하여 균등 분포가 가능하다.      
> **단점**: 충돌 가능성이 여전히 존재하고, 큰 숫자나 긴 문자열의 경우 오버플로우 주의가 필요하다.

**예시 1 - 문자열**

```python
def hash_digit_folding_string(key, table_size):
  # 각 문자를 ASCII로 변환하여 합산
  total = sum(ord(char) for char in key)
  return total % table_size

# 결과
# "hello"의 경우
# h(104) + e(101) + l(108) + l(108) + o(111) = 532
print(has_digit_folding_string("hello", 13))  # 532 % 13 = 12
```
**예시 2 - 전화번호(숫자)**

```python
def hash_digit_folding_number(phone_number, table_size):
  # 전화번호를 2자리씩 나누어 합산
  phone_str = str(phone_number)
  total = 0

  for i in range(0, len(phone_str), 2):
    chunk = phone_str[i:i+2]
    total += int(chunk)

  return total % table_size

# 결과
# "5551234567"의 경우
# 55 + 51 + 23 + 45 + 67 = 241
print(hash_digit_folding_number(5551234567, 13))  # 241 % 13 = 7
```

### Multiplication Method(곱셈 법)
키(key)에 **0과 1 사이의 상수 A**를 곱한 후, **소수 부분만 추출**하여 해시 값을 생성

```python
h(k) = ⌊m × (k × A mod 1)⌋
```

#### 바닥 함수 (`⌊x⌋`) & 천장 함수 (`⌈x⌉`)
* 바닥 함수 (`⌊x⌋`)
  * x보다 작거나 같은 가장 큰 정수입니다.         
  * 예시: ⌊4.9⌋ = 4, ⌊5⌋ = 5, ⌊-0.1⌋ = -1       
* 천장 함수 (`⌈x⌉`)
  * x보다 크거나 같은 가장 작은 정수입니다.           
  * 예시: ⌈3.1⌉ = 4, ⌈7⌉ = 7, ⌈-2.5⌉ = -2           

> 테이블 크기 m은 **2의 제곱수를 사용해도 무방**      
> A 값으로는 **황금비율의 역수** `(√5 - 1) / 2 ≈ 0.6180339887`를 주로 사용      
> Division Method보다 충돌이 적다.

**예시**

```python
import math

def hash_multiplication(key, table_size):
  A = (math.sqrt(5) - 1) / 2  # 황금비율의 역수
  fractional_part = (key * A) % 1
  return math.floor(table_size * fractional_part)

# 결과
print(hash_multiplication(123, 100)) # 18
print(hash_multiplication(456, 100)) # 75
```
### Univeral Hashing(보편 해싱)
**해시 함수들의 집합(family) H**를 미리 정의하고, 실행 시점에 **무작위로 하나의 해시 함수를 선택**하여 충돌을 최소화하는 방법이다.

```python
h(k) = ((a * k + b) mod p) mod m
```
* **p**: 테이블 크기보다 큰 소수
* **a, b**: 1 <= a < p, 0 <= b < p 범위의 무작위 정수
* **m**: 테이블 크기

> 최악의 경우를 대비한 **확률적 접근법**        
> 악의적인 입력 패턴에 강하다.        
> 매번 다른 해시 함수를 사용하여 **보안성 향상**        

**예시**

```python
import random

class UniversalHashing:
  def __init__(self, table_size):
    self.m = table_size
    self.p = 109345121 # 큰 소수
    self.a = random.randint(1, self.p - 1)
    self.b = random.randint(0, self.p - 1)

  def hash(self, key):
    return ((self.a * key + self.b) % self.p) % self.m

uh = UniversalHashing(13)
# 결과
print(uh.hash(100)) # 매번 다른 결과
```

# 해쉬 충돌(Hash Collision)이란?
**서로 다른 키(Key)가 같은 해시 값(인덱스)을 가지는 경우**를 의미한다.

## 충돌이 발생하는 이유
![Hash Collison](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fd0a1LP%2FbtrJ7Ool5g4%2FAAAAAAAAAAAAAAAAAAAAAEWnJNwmSCsDNievL13_s3uCdOBC55m_llIEkmbB3Svo%2Fimg.jpg%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DEegfE841N6EmP8IdPLKqLvewcfg%253D)

> 그런데 만약 사과와 배를 해시 함수를 돌렸을 때 나온 해시 값이 동일하다면, 이때 **충돌**이 발생하게 된다.     
> 이것을 해시 충돌(Hash Collison)이라고 말하며 이를 해결하기 위한 방법이 여러가지 있다.

## 왜 충돌은 불가피할까?
### 비둘기집 원리(Pigeonhole Principle)
"`n`개의 비둘기집에 `n + 1`마리 이상의 비둘기를 넣으면, 반드시 한 집에 2마리 이상이 들어간다."

> 무한한 키 공간을 유한한 배열 크기로 매핑하기 때문에     
> n개의 버킷에 n + 1개 이상의 키를 저장하면 오버플로우가 발생하여 반드시 충돌이 발생한다

### 해시 함수의 한계
"**완벽하게 균등 분포를 보장하는 해시 함수는 존재하지 않는다.**"

> 위의 사과와 배처럼 해시 함수를 돌렸을 때 나온 해시 값이 늘 다를 수는 없다.      
> 즉, 입력 데이터의 특성에 따라 충돌 발생이 가능하다.

## 충돌 VS 오버플로우
**충돌을 해결하기 위한 방법을 알아보기전, 정확히 충돌과 오버플로우의 차이를 짚고 넘어가자.**    

**충돌(Collision)이란?**    
* 서로 다른 키가 같은 인덱스로 매핑되는 현상(같은 집을 구매한 주민)
 
**오버플로우(Overflow)란?**
* 테이블이 가득 차서 더 이상 저장할 공간이 없는 상태(집에 방이 없을 때 상태)

### 충돌이 먼저, 오버플로우는 나중
**충돌은 있지만 오버플로우는 없음**

```python
table = [None] * 10
table[3] = 'apple'  # 1/10 사용

# banana도 해시값 3 -> 충돌
# 하지만 다른 슬롯은 비어있음 (9개 남음)
# -> 다른 곳에 저장 가능 (개반 주소법)

table[4] = 'banana' # 2/10 사용
# 충돌은 있었지만 오버플로우는 아님
```
**충돌이 쌓여서 오버플로우 발생**

```python
table = [None] * 3
table[0] = 'apple'  # 1/3
table[1] = 'banana' # 2/3
table[2] = 'orange' # 3/3 (가득 찼지만 아직 OK)

# melon의 해시값이 0이라면?
# 0 -> 충돌 -> 1로 이동 -> 충돌 -> 2로 이동 -> 충돌
# -> 모든 슬롯이 차있음 -> 오버플로우!
```
### 충돌 vs 오버플로우 차이표

| 구분 | 충돌(Collision) | 오버플로우(Overflow) |
|:------:|----------------|-------------------|
| **정의** | 서로 다른 키가 같은 인덱스 | 테이블이 가득 참 |
| **발생 원인** | 해시 함수의 특성 | 공간 부족 |
| **발생 시점** | 언제든지 가능 | 테이블이 거의 찼을 때 |
| **테이블 상태** | 빈 공간 있을 수 있음 | 빈 공간 없음 |
| **해결 방법** | 체이닝, 개방 주소법 | 리사이징(확장) |

## Chaining 기법
![chaining](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fs61LS%2FbtrJ8ndq6mM%2FAAAAAAAAAAAAAAAAAAAAALGKB81xW9tZtcI1KONPwC2JCRqx87rR2bCS-AluYEZI%2Fimg.jpg%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DGKHW5AVzkIQ%252Fh4loUq9gG4Gsc%252FE%253D)

> Chaining 기법은 Open Hashing 기법 중 하나로,    
> **해쉬 테이블 저장공간 이외의 공간을 활용하는 기법**이다.     
> 즉, 충돌이 일어나게 되면 해당 인덱스가 가리키는 해쉬 테이블 공간 뒤로 **연결 리스트**를 사용하여 추가적으로 연결시킨 다음 저장한다.(동일 버켓(주소)의 슬롯을 늘리는 것)

## 개방주소법(Open Addressing)
### 선형 조사법(Linear Probing)
![linearProbing](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fd0a1LP%2FbtrJ7Ool5g4%2FAAAAAAAAAAAAAAAAAAAAAEWnJNwmSCsDNievL13_s3uCdOBC55m_llIEkmbB3Svo%2Fimg.jpg%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DEegfE841N6EmP8IdPLKqLvewcfg%253D)

> Linear

### 이차 조사법(Quadratic Probing)

### 이중 해싱법(Double Hashing)

### 뻐꾸기 해싱(Cuckoo Hashing)

## 재해싱(Rehashing)


### 좋은 해시 함수의 조건
> **계산이 빠를 것**
* 해시 함수 자체가 복잡하면 O(1)의 장점이 사라짐      
* 단순한 산술 연산으로 구현되어야 함  

> **균등 분포(Uniform Distribution)**
* 키들이 해시 테이블 전체에 고르게 분산되어야 함      
* 특정 버킷에만 올리면 성능 저하 발생     

> **충돌 최소화**
* 서로 다른 키가 같은 해시 값을 가질 확률을 최소화
* 완벽한 해시 함수는 불가능하지만 충돌을 줄이는 것이 중요

> **결정론적(Deterministic)**
* 같은 키는 항상 같은 해시 값을 반환해야 함
* 일관성이 보장되어야 데이터 검색 가능 

> **눈사태 효과(Avalanche Effect)**
* 입력의 작은 변화가 출력에 큰 변화를 일으킴
* 비슷한 키들이 서로 다른 해시 값을 가지도록 함
