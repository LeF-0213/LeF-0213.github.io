---
layout: post
related_posts:
    - /studylog/java
title:  "자바 라이브러리에 대한 이해"
date:   2025-05-04
categories:
  - studylog
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