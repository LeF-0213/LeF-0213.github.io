---
layout: post
related_posts:
    - /baekend/java
    - /baekend/java/2025-03-14-Access-Modifiers
    - /baekend/java/2025-03-29-Inner-Class
title:  "자바 외부 클래스"
date:   2025-03-29
categories:
  - baekend
  - java
description: >
  Java의 외부 클래스의 종류와 특징
---
* toc
{:toc .large-only}

# 외부 클래스(OuterClass)
* 가장 일반적인 형태의 클래스, 독립적으로 존재가능
* 내부 클래스를 포함할 수 있으며, 내부 클래스에서 접근 가능한 멤버(필드, 메서드)를 가질 수 있다.
```java
    // 외부 클래스
    public class OuterClass {
        // 필드와 메소드

        // 내부 클래스
        class InnerClass {
            // 내부 클래스의 필드와 메서드
        }
    }
```