---
layout: post
related_posts:
    - /baekend/java
title:  "자바 라이브러리에 대한 이해"
date:   2025-06-21
categories:
  - baekend
  - java
description: >
  자바의 대표적인 라이브러리 및 활용 예시
---
* toc
{:toc .large-only}

# 라이브러리란?
개발자가 자주 사용하는 함수, 클래스, 모듈 등 **미리 만들어 놓은 코드 집합**을 말한다. 쉽게 말해 `누군가가 만들어 놓은 유용한 도구 상자`라고 할 수 있다.

## 라이브러리의 주요 특징
* 재사용 가능: 이미 만들어진 기능을 써서 개발 속도를 높일 수 있음
* 검증된 코드: 이미 검증된 코드라 신뢰성이 높음
* 생산성 향상: 처음부터 구현하지 않아도 되서 개발 시간이 절약됨

## 자바에서의 라이브러리란?
자바에서 라이브러리는 `.jar`(Java ARchive) 파일 형태로 제공

# 자바에서 지원하는 라이브러리


# 🔖 활용 예시
```java
    package net.lef.util;
    import java.io.*;

    public class Print {
        public static void print(Object obj) {
            System.out.println(obj);
        }

        public static void print() {
            System.out.println();
        }

        public static void printnb(Object obj) {
            System.out.print(obj);
        }

        public static PrintStream printf(String format, Object... args) {
            return System.out.printf(format, args);
        }
    }
```

```java
    import static net.lef.util.Print.*;

    public class PrintTest {
        public static void main(String[] args) {
            print("print util test");
            print(30);
            print(30.23);
        }
    }
```
**사용자 정의 라이브러리를 사용할 때에는 반드시 `static` 키워드를 붙인다.**

## JAVA 내장 API
### Object 클래스
java의 최상위 클래스  
java의 모든 타입(클래스)은 Object 클래스를 상속받아서 만들어져있다.

### toString 메소드
객체를 출력했을 때의 모양을 설정하는 메소드  
기본적으로 Object클래스에서 toString은  
`프로젝트폴더이름.객체이름@위치정보`로 설정되어있다.

### equals 메소드
객체의 물리적인 주소가 서로 같은지 비교하는 것이 아니라,  
객체의 논리적인 내용이 서로 같은지 비교  

**비교하고 싶을 때 사용하는 메소드**
```java
// ex) a <-- (국어점수90인 학생정보 가져다줘)  
// 		 c <-- (학생번호1번인 학생정보 가져다줘)  
// a와 c는 논리적으로 동일한 학생 정보 
a.eqauls(c);
```

### hashcode 메소드
집합을 구현할 때 **hashcode가 같은 객체**는  
서로 동일한 객체라고 판단하게 되어있다.  
따라서 논리적으로 같은 객체일 경우,  
같은 hashcode가 나오도록 메소드를 재정의한다.

객체를 만들때
객체를 출력했을 때의 결과를 정의하고 싶다면
toString 메소드를 오버라이딩한다

객체가 논리적으로 서로 일치하는지 확인하고 싶다면
equals 메소드를 오버라이딩한다.

일반적으로 equals메소드를 오버라이딩하면
hashCode메소드도 오버라이딩하여 동일한 객체임을 알려준다.

String 클래스
문자열에서 사용할 수 있는 여러개의 기능을 메소드로 만들어 놓았다.

**length**
1. 인자 X
2. 문자열의 길이를 알려준다.
3. int값

**indexOf**
1. 문자열 / 문자열, int값
2. 해당문자열을 0번부터 찾는다 / 문자열을 int값부터 찾는다.
3. int값(위치 index)

**charAt**
1. int값
2. 매개변수로 받은 int값 위치에 존재하는 문자가 뭔지 찾는다
3. char값(해당 위치에 있는 문자)

**split**
1. String값(쪼갤 기준)
2. 매개변수로 받은 String값을 기준으로 쪼개서 배열을 만든다
3. String[] (String이 요소로 들어있는 배열)

**join(static 메소드)**
1. String, String[]
2. 매개변수로 전달받은 String[]을 String값을 기준으로 묶어서 하나의 문자열을 만든다
3. String값

**substring**
1. int값
2. 매개변수로 전달받은 int값 index부터 끝까지 잘라내는 메소드
3. String값(잘린 결과)
