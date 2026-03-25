---
layout: post
related_posts:
    - /frontend/python
title:  "[자료 구조]해시 테이블(Hash Table)과 해시 함수"
date:   2025-10-29
categories:
  - frontend
  - python
description: >
  해시 테이블의 개념과 해시 충돌이 왜 발생하는지 그 해결방안은 무엇인지에 대한 정리
---
* toc
{:toc .large-only}

## 해시(Hash)란?

> 임의의 길이의 데이터를 입력받아 **고정된 길이의 데이터**로 변환하여 출력한 **결과 값 자체**를 의미한다.     
> 이 결과 값은 **해시 값(Hash Value), 해시 코드(Hash Code)** 또는 **다이제스트(Digest)**라고도 불린다.

## 해싱(Hashing)이란?

> **해싱(Hashing)**은 이 해시 값을 생성하는 **변환 과정 전체**를 의미하며, 이 과정을 수행하는 함수를 **해시 함수(Hash Function)**라고 한다.

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

### 해시 함수의 특징
* **단방향성(One-way Function)**: H(x)를 통해 얻은 Y값으로 다시 X값을 얻을 수 없음
* **결정론적(Deterministic)**: 같은 입력(M)은 항상 같은 해시 값(H) 출력을 생성
* **충돌 가능성**: 다른 입력 X1, X2가 같은 출력 Y를 만들 수도 있음 
-> **해시 충돌(Hash Collision)** 발생
* **고정된 길이의 출력(Fixed-size-Output)**: 입력 데이터의 길이와 상관없이, 해시 함수를 통과하면 항상 **미리 정해진 길이**의 출력값(ex: SHA-256은 256비트)을 생성한다.
  
### 해싱의 주요 활용 분야

> **비밀번호 저장(Password Storage)**: 사용자의 비밀번호를 해싱하여 저장함으로써, 데이터베이스가 해킹당해도 원본 비밀번호를 보호한다.       
> **무결성 검증(Data Integrity)**: 데이터의 해시 값을 비교하여, 파일이나 메시지가 전송 또는 저장과정에서 **변조되지 않았음**을 확인한다. 블록체인 기술의 핵심 요소이다.     
> **자료구조(Hash Table)**: **해시 테이블(Hash Table)**에서 키(Key)를 배열의 인덱스로 변환하여 데이터를 저장하고 검색함으로써 **매우 빠른 데이터 접근 속도(평균 O(1))**를 가능하게 한다.

## 해쉬 함수의 종류
### Division Method(나눗셈 법)
입력값을 테이블 크기로 나눈 **나머지**를 해시 값으로 사용하는 방법    

```python
h(k) = k mod m
```   
**특징:**

> 가장 간단하고 빠른 방법     
> 테이블 크기 m은 **소수(prime number)**를 사용하는 것이 좋다.      
> **2의 제곱수**는 피하는 것이 좋다.(비트 패턴에 의존하기 때문)

**예시**

```python
def hash_division(key, table_size):
  return key % table_size

# table_size = 13(소수)
print(hash_division(25, 13)) # 12
print(hash_division(38, 13)) # 12 (충돌 발생)
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
**예시**

```python
import math

def hash_multiplication(key, table_size):
  A = (math.sqrt(5) - 1) / 2 # 황금비율의 역수 ≈ 0.618
  fractional_part = (key * A) % 1
  return math.floor(table_size * fractional_part)

print(hash_multiplication(123, 100))  # 18
print(hash_multiplication(456, 100))  # 75
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
## 암호화 해시 함수
일반 해시 함수와 달리, **보안이 강화된 특수 해시 함수**로 비밀번호 저장, 데이터 무결성 검증 등에 사용한다.

### SHA-256(Secure Hash Algorithm 2)

> 출력: 256비트(64자 16진수)      
> **매우 강력한 보안** -> 블록체인, 비밀번호 저장에 광범위하게 사용한다.      
> 작은 입력 변화도 완전히 다른 출력 생성(눈사태 효과)

```python
import hashlib

text = "Hello World"
sha246_hash = hashlib.sha256(text.encode()).hexidigest()
print(sha256_hash)
# a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad6f146e

# 비교: 입력 변화가 출력을 완전히 변경
text2 = "Hello World!"  # 느낌표 하나만 추가
sha256_hash2 = hashlib.sha256(text2.encode()).hexdigest()
print(sha256_hash2)
# 7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069
```

**활용**

* 블록체인 (Bitcoin, Ethereum)
* SSL/TLS 인증서
* Git 커밋 검증

### SHA-512(Secure Hash Algorithm 2-512)

> 출력: 512qlxm (128자 16진수)        
> SHA-256보다 더 긴 출력 → 극도로 강력한 보안       
> 매우 민감한 정보 보호에 사용

```python
import hashlib

text = "Hello World"
sha512_hash = hashlib.sha512(text.encode()).hexdigest()
print(sha512_hash)
# f910d0ccd3d42ab0e2cdc5760245c4dd5b0a9ae7acb76c7b1bc8e989e005bca02
# d87e1e43487b3c6e76dfbed69b51ae269dd2e6c5949faea2c282622faf553145

# 512비트 = 매우 큰 공간 = 충돌 가능성 극히 낮음
```
**활용**

* 매우 민감한 데이터 보호
* 금융/보안 시스템
* 보안 저장소

### bcrypt - 🔐 비밀번호 전문

> 범용 해시 함수가 아닌 **비밀번호 저장용 해시 함수**     
> **Salt와 반복 횟수**를 포함 -> 브루트포스 공격 방어     
> 계산이 의도적으로 느림(보안 강화)

```python
import bcrypt

password = "password123"

# 해시 생성 (자동으로 salt 포함)
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
print(hashed)
# b'$2b$12$abcdefg...'

# 비밀번호 검증
is_correct = bcrypt.checkpw(password.encode(), hashed)
print(is_correct)  # True

wrong_password = "wrongPassword"
is_correct = bcrypt.checkpw(wrong_password.encode(), hashed)
print(is_correct)  # False
```

## 해쉬 충돌(Hash Collision)이란?
**서로 다른 키(Key)가 같은 해시 값(인덱스)을 가지는 경우**를 의미한다.

### 충돌이 발생하는 이유
![Hash Collison](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fd0a1LP%2FbtrJ7Ool5g4%2FAAAAAAAAAAAAAAAAAAAAAEWnJNwmSCsDNievL13_s3uCdOBC55m_llIEkmbB3Svo%2Fimg.jpg%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DEegfE841N6EmP8IdPLKqLvewcfg%253D)

> 그런데 만약 사과와 배를 해시 함수를 돌렸을 때 나온 해시 값이 동일하다면, 이때 **충돌**이 발생하게 된다.     
> 이것을 해시 충돌(Hash Collison)이라고 말하며 이를 해결하기 위한 방법이 여러가지 있다.

### 왜 충돌은 불가피할까?
#### 비둘기집 원리(Pigeonhole Principle)
"`n`개의 비둘기집에 `n + 1`마리 이상의 비둘기를 넣으면, 반드시 한 집에 2마리 이상이 들어간다."

> 무한한 키 공간을 유한한 배열 크기로 매핑하기 때문에     
> n개의 버킷에 n + 1개 이상의 키를 저장하면 오버플로우가 발생하여 반드시 충돌이 발생한다

#### 해시 함수의 한계
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

> 추가적인 메모리 공간을 사용하지 않고 **이미 확보된 공간만 사용하는 방식**이다.      
> 즉, 충돌이 발생했다면 인접한 비어있는 공간을 활용한다.      
> 예시는 복잡합을 줄이기 위해 전부 key값과 value값이 같다고 가정한다.

### 선형 조사법(Linear Probing)
![lp](https://github.com/user-attachments/assets/f1f3cade-feff-4824-bd64-f51085885bd8)

```python
hf_lp(key, i) = (hf(key) + i)
# i는 충돌 횟수로 1씩 증가한다, m은 해시 테이블 크기
```

> 선형 조사법은 **충돌이 발생했을 때, 그 다음 빈공간을 찾아 순차적으로 이동하는 것**이다.     
> 만약 해시 함수를 수행한 값이 2가 나오면 이미 50이 들어있어 충돌이 일어나기에,      
> 2 + 1 = 3이 되고, 또 55와 충돌이 일어나게 되기에      
> 3 + 1 = 4에 값을 집어넣는다.        
> 이런식으로 순차적으로 빈공간을 찾아가기에       
> 값들이 뭉쳐있는 **군집화(Clustering)**가 발생한다.

### 이차 조사법(Quadratic Probing, 제곱 함수법)
![qp](https://github.com/user-attachments/assets/60d3e863-ec41-4346-a905-78ab4dafe2a1)

```python
hf_qp(key, i) = hf(key) + i^2 
# i^은 더 복잡하게 c*i^ 혹은 c1*i^2 + c2*i로 바꿀 수 있다. 
# 여기서 c는 상수를 의미한다.
```

> 충돌이 되면 다시 해시 함수를 수행하고 충돌이 나지 않는 해시 값에 해당하는 버켓에 넣어준다.      
> 예를 들면, 위의 이미지의 초기상태에서 35를 삽입하고자 한다.     
> hf(35)의 값이 9가 나왔다.         
> 그러나 9에는 이미 22가 들어있어 다시 충돌횟수를 1 늘려 이차 조사법을 수행한다.       
> 9 + 1^2 = 10이 된다. 10 % 12 = 10이고, 10은 빈 버켓이므로 35를 넣어준다.      
> 이런 식으로 반복해 빈 버켓을 찾아 반복하여 값을 넣어준다.       
> 군집현상이 발생한 공간에 빠졌을 때 빨리 탈출하기 위해서 제곱수를 사용한다.

### 이중 해싱법(Double Hashing)
![dh](https://github.com/user-attachments/assets/2d89a649-3b87-4635-ae30-97b70876fb8b)

```python
hf_dh(key, i) = hf(key) + i * 2nd_hf(key)
# ⚠️ 주의점: 2nd_hf(key)의 값이 map 사이즈와 서로소여야 한다.
# 그렇지 않으면 한번도 접근하지 못하는 공간(bucket) 발생한다.
```

> hf_dh(key, i) = hk(key)를 수행하게 되었을 때 충돌이 일어나면,       
> 빈공간을 찾을 때 2nd_hf(key) 또 다른 해시함수를 사용하면 더 잘 퍼뜨릴 수 있기에 사용한다.         
> 이차조사법(Quadratic Probing) 같은 경우는 해시 함수를 돌렸을 때,        
> 나**오는 값이 같으면 아무리 충돌 횟수가 늘어나도 결국 같은 위치를 계속 확인할 수 밖에 없다**는 단점이 있다.       

> 이런 부분을 해결하기 위해 이중 해싱법을 사용하는 것이다.        
> 하지만 2nd_hf(key) 즉, **두번째 해시함수를 수행해서 나온 값이 map의 사이즈와 서로소야 한다.**
> 만약 map_size가 10이고, 2nd_hf(key)가 4, hf(key)가 0이면,     
> 결국 hf_dh(key, i) = 0 + i * 4이므로,         
> 4의 배수값을 map_size로 나눈 4, 8, 2, 6, 0 만 출력하게 된다.
> 때문에 **서로소이지 않으면 한번도 접근하지 못하는 공간이 발생**하는 것이다.

### 뻐꾸기 해싱(Cuckoo Hashing)
<table>
<tr>
<td><img src="https://github.com/user-attachments/assets/dc43e497-be1a-42e0-a999-ee780b6b1bf5" width="330" height="300"/></td>
<td><img src="https://github.com/user-attachments/assets/39940ca0-5e70-4cd9-8435-c5d47af4ae46" width="330" height="300"/></td>
<td><img src="https://github.com/user-attachments/assets/a6635255-5081-4804-8bc3-56a1f5cc9271" width="330" height="300"/></td>
</tr>
</table>

### 재해싱(Rehashing)

## Oppen addressing 주의점
Oppen addressing은 **이미 있는 공간만으로 해시충돌을 해결하는 방식**이기에,     
**중간 연결고리 역할을 하는 key의 삭제 시, 이미 있는 값도 없다고 판단할 수 있다.**

![lpdelete](https://velog.velcdn.com/images%2Flacomaco%2Fpost%2F27497008-0d87-4062-8e9d-4335dc1f7970%2F%EC%84%A0%ED%98%95.png)

> 위의 선형 조사법(Linear Probing) 이미지에 맞춰 이야기하면,    
> 원래는 인덱스 2의 위치에 15가 들어있어 28은 충돌을 피해서 인덱스 4번에 위치해 있다.     
> 그러나 만약 인덱스 2번의 값을 삭제하고,         
> 다음에 28이 다시 입력이 되면 인덱스 2번에도 28, 인덱스 4번에도 28이 들어있게 된다.      
> 즉, 다음 공간을 확인하지 않고 없을 것이라 가정하고 값을 집어넣는 버그가 발생할 수도 있다.     

### 해결책
**DELETE로 마킹하는 방식**

> 삭제 시 DELETE 같은 상징적인 형태로 표시      
> 따라서 DELETE 표시를 보고 다음 위치도 확인해봐야함을 알 수 있다.      
> 그런데 문제는 DELETE를 만나면 내가 찾는 값이 들어있을 수 있기 때문에      
> 무조건 다음 위치를 확인해 봐야 된다.    
> 즉, 중복되는 값이 없을 때도 DELETE를 만나면 끝까지 확인하게 되기 때문에,      
> 추가적인 오퍼레이션이 발생한다.

**Open Addressing된 Key들을 한 단계씩 앞으로 옮겨주는 방식**

> **삭제 위치 다음의 Open addressing된 key들은 한 단계씩 앞으로 옮겨준다.**     
> 대신 **이동시키는 추가적인 비용이 발생**된다.       
> 예를 들어 그림에서 인덱스 6번의 38을 삭제하면,      
> 다음 위치에 있던 7과 20을 한 단계씩 끌어와      
> 인덱스 6번에 7, 인덱스 7번에 20이 위치하게 된다.   

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