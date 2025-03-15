---
layout: post
title:  "자바 기본 문법"
date:   2025-03-04
categories: jekyll update
---
# 자바 기본 문법

## 프로그램의 기본 구조 vs 자바 프로그램의 기본 구조
![자바 컴파일 과정](https://velog.velcdn.com/images/stemmmm/post/d32fdb27-b78f-4815-b5f7-0dd1a36b1b9b/image.png)

## 플랫폼에 독립적인 자바

Java는 **Write Once, Run Anywhere(WORA)** 개념을 따릅니다. 이는 Java 코드가 특정 운영체제에 종속되지 않고 다양한 환경에서 실행될 수 있음을 의미합니다.

### C 프로그램 vs 자바 프로그램
- **C 프로그램**: 바로 기계어로 컴파일하므로 HW 운영체제에 맞게 각각 컴파일 필요 → **플랫폼에 종속적**
- **자바 프로그램**: 중간 단계 언어로 컴파일하여 JVM만 있으면 어디서든 실행 가능 → **플랫폼에 독립적**

### 자바의 동작 원리
- **Java 컴파일러(javac)**: Java 소스 코드(`.java` 파일)를 **바이트코드(ByteCode)** (`.class` 파일)로 변환
- **Java 바이트코드**: 
  - 운영체제에 종속되지 않는 중간 코드
  - 명령어 크기가 1byte라서 bytecode라고 불림
  - `.class` 확장자를 가짐
  - JVM이 설치된 어떤 운영체제에서도 실행 가능
- **Java 가상머신(JVM)**: 
  - 바이트코드를 해석하고 실행
  - 메모리 관리 및 자원 할당
  - 각 운영체제별로 다른 버전 제공

## 자바 프로그램의 기본 구조

### 소스 파일 구성요소

구성요소 | 설명 | 예시
---------|------|------
**소스 파일** | 자바 코드가 저장된 파일 | `Main.java`
**패키지(Package)** | 관련 클래스들을 그룹화 | `package com.example.myapp;`
**임포트(Import)** | 다른 패키지의 클래스 사용 선언 | `import java.util.ArrayList;`
**클래스(Class)** | 객체의 설계도 | `public class ClassName {...}`
**메서드(Method)** | 클래스 내의 기능 구현 | `public void methodName() {...}`

### 패키지(Package) 선언
```java
// 회사 도메인 기반 패키지
package com.example.myapp;

// 오픈소스 프로젝트 패키지 예시
package org.project.util;
```

### 임포트(Import) 선언
```java
// 특정 클래스 임포트
import java.util.ArrayList;

// 패키지 내 모든 클래스 임포트
import java.util.*;
```

### 클래스(Class) 선언
```java
public class ClassName {
    // 필드(클래스 변수, 인스턴스 변수)
    // 생성자
    // 메서드
}
```

### 메서드(Method) 선언
```java
접근제어자 [static] 반환타입 메서드명(매개변수타입 매개변수명, ...) {
    // 실행문
    return 반환값;  // void가 아닐 경우
}
```

## 주석(Comments)

주석 유형 | 문법 | 용도
---------|------|------
**한 줄 주석** | `// 주석 내용` | 간단한 설명이나 임시 코드 비활성화
**여러 줄 주석** | `/* 주석 내용 */` | 여러 줄에 걸친 설명이나 코드 블록 비활성화
**문서화 주석** | `/** 주석 내용 */` | JavaDoc 도구로 API 문서 자동 생성용

## 명명 규칙(Naming Conventions)

대상 | 규칙 | 예시
------|------|------
**클래스** | 파스칼 케이스(PascalCase), 명사 | `StudentManager`, `BankAccount`
**인터페이스** | 파스칼 케이스, 형용사 | `Runnable`, `Comparable`
**메서드** | 카멜 케이스(camelCase), 동사 | `calculateTotal()`, `sendEmail()`
**변수** | 카멜 케이스, 명사 | `studentCount`, `firstName`
**상수** | 대문자 스네이크 케이스 | `MAX_SIZE`, `DEFAULT_TIMEOUT`
**패키지** | 모두 소문자, 도메인 역순 | `com.company.project`

## 식별자 규칙

허용 | 규칙
-----|------
✓ | 영문자, 숫자, `$`, `_`만 사용 가능
✓ | 대소문자 구분
✗ | 숫자로 시작 불가능
✗ | 예약어 사용 불가능

## 예약어 목록
```
abstract   continue   for         new         switch
assert     default    goto        package     synchronized
boolean    do         if          private     this
break      double     implements  protected   throw
byte       else       import      public      throws
case       enum       instanceof  return      transient
catch      extends    int         short       try
char       final      interface   static      void
class      finally    long        strictfp    volatile
const      float      native      super       while
```