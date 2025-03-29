---
layout: post
related_posts:
    - /studylog/java
title:  "자바 접근 제어자"
date:   2025-03-14
categories:
  - studylog
  - java
description: >
  Java의 접근 제어자의 종류와 개념
---
* toc
{:toc .large-only}

# 자바의 접근 제어자(Access Modifiers)
접근 제어자는 클래스와 클래스 멤버(변수, 메서드, 생성자)의 접근 범위를 제한하여 캡슐화(Encapsulation)와 정보 은닉(Information Hiding)을 구현하는데 중요한 역할을 한다. 보통 접근제어자 또는 접근지정라고 부르며, 클래스나 클래스 멤버 앞에 붙어있는 public, private, protected 등의 키워드가 바로 접근제어자이다. 

## 핵심 개념
자바에는 4가지 접근 제어자가 있으며, 접근 범위가 넓은 것부터 좁은 순으로 나열하면 다음과 같다.
1. **public**: 접근 제한이 없음
2. **protected**: 동일 패키지 내 또는 상속 관계의 클래스에서 접근 가능
3. **default**(접근 제어자 선언 생략): 동일 패키지 내에서만 접근 가능
4. **private**: 동일 클래스 내에서만 접근 가능

## 접근 제어자 적용 가능 요소
* 클래스(Class): public, default
* 멤버 변수(Field): public, protected, default, private
* 메서드(Method): public, protected, default, private
* 생성자(Constructor): public, protected, default, private
* 지역 변수(Local Variable): 접근 제어자 적용 불가

### 접근 범위 비교

