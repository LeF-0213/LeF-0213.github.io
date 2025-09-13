---
layout: post
related_posts:
    - /studylog/errorLog
title:  "error: no suitable method found for sort(int[],Comparator<Object>)"
date:   2025-09-05
categories:
  - studylog
  - errorLog
  - java
description: >
  Collections.reverseOrder() 사용시 타입 호환 에러
---
* toc
{:toc .large-only}

# 문제 발생
`error: no suitable method found for sort(int[],Comparator<Object>)`
```txt
java: no suitable method found for sort(int[],java.util.Comparator<java.lang.Object>)
method java.util.Arrays.sort(T[],java.util.Comparator<? super T>) is not applicable
(inference variable T has incompatible bounds
equality constraints: int
lower bounds: java.lang.Object)
method java.util.Arrays.sort(T[],int,int,java.util.Comparator<? super T>) is not applicable
(cannot infer type-variable(s) T
(actual and formal argument lists differ in length))
```
## 문제 원인
* Arrays.sort(T[], Comparator<? super T>) 메서드는 객체 배열(T[])에만 적용된다. 
* Java의 int는 원시 타입(Primitive Type)이기에 호환되지 않는다. 
* `Collections.reverseOrder()`는 Comparator 인터페이스를 구현하며, 객체 타입의 배열에만 적용할 수 있습니다. 
## 해결 방안
int형 배열을 Integer형 배열로 수정함으로 해결할 수 있다.