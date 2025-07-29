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
  
---
* toc
{:toc .large-only}

## 언어
- 절차적언어
- 객체지향언어
=> 

## 자동형변환
데이터형(type)
정수(byte, int) 실수(float, double) 문자(char) 문자열(String)
```python
  a = 10;
  a = 10.5;
  a = 'A'
```


## 변수란?
변수 = 단수데이터
1. 변수 이름은 문자, 숫자, 언더바(_)만 사용 가능
    ```txt
    !num(X) _num(O)
    1_num(X) num_1(O)
    ```
2. 
3. 

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
print("%d + %d" % (100, 100))