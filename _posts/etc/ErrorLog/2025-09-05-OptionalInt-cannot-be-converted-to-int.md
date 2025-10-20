---
layout: post
related_posts:
    - /etc/errorlog
title:  "incompatible types: OptionalInt cannot be converted to int"
date:   2025-09-05
categories:
  - etc
  - errorlog
  - java
description: >
  [Vue.js] Parsing error: No Babel config file detected 에러 해결방법
---
* toc
{:toc .large-only}
# error: incompatible types: OptionalInt cannot be converted to int

## 문제발생 
```java
int[] arr = 
int max = Arrays.stream(arr).max(); // OptionalInt 타입
```

## 문제원인
getAsInt 함수는 Java Stream API의 최종 연산자로, IntStream에서 마지막 요소를 정수 값으로 가져오는 데 사용됩니다. OptionalInt 타입으로 반환되는데, 이는 스트림에 요소가 있을 수도 있고 없을 수도 있기 때문이며, `getAsInt()`를 호출하면 OptionalInt에 값이 존재할 경우 해당 정수 값을 반환하고, 값이 없을 경우 NoSuchElementException을 발생시킵니다. 

## 해결방안
```java
int[] arr = 
int max = Arrays.stream(arr).max().getAsInt(); // int타입으로 반환
```
