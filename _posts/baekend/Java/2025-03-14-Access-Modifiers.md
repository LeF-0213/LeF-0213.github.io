---
layout: post
related_posts:
    - /studylog/java
title:  "자바 접근 제어자(Access Modifiers)"
date:   2025-03-14
categories:
  - studylog
  - java
description: >
  Java의 접근 제어자의 종류와 개념
---
* toc
{:toc .large-only}

# 접근제어자(Access Modifiers)란?
접근 제어자는 클래스와 클래스 멤버(변수, 메서드, 생성자)의 접근 범위를 제한하여 캡슐화(Encapsulation)와 정보 은닉(Information Hiding)을 구현하는데 중요한 역할을 한다. 보통 접근제어자 또는 접근지정라고 부르며, 클래스나 클래스 멤버 앞에 붙어있는 public, private, protected 등의 키워드가 바로 접근제어자이다. 

## 접근 제어자 종류
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

### 접근 제어자 접근 범위 비교
![JavaAccessModifier](https://github.com/user-attachments/assets/c9b507eb-1c83-4029-9f3a-fa6b4ddae2d7)
| 접근 제어자 | 같은 클래스 | 같은 패키지 | 하위 클래스 | 다른 패키지 |
|------------|:----------:|:----------:|:----------:|:----------:|
| public     | ✓          | ✓          | ✓          | ✓          |
| protected  | ✓          | ✓          | ✓          | ✗          |
| default    | ✓          | ✓          | ✗          | ✗          |
| private    | ✓          | ✗          | ✗          | ✗          |

```java
  Phone 객체  = new Phone(); // 객체를 초기화(생성자)

  Phone p1 = new Phone(); // 디폴트 생성자
  Phone p2 = new Phone("test");
  Phone p3 = new Phone(10);
```


## 접근 제어자의 특징과 용도
* **캡슐화(Encapsulation)**: 객체의 내부 상태를 외부로부터 보호하여 데이터 무결성 유지
* **정보 은닉(Information Hiding)**: 필요한 정보만 공개하고 내부 구현을 숨겨 유지보수성 향상
* **접근 범위**: public > protected > default > private 순으로 범위가 좁아짐
