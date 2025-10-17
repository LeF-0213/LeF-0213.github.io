---
layout: post
related_posts:
    - /studylog/java
    - /studylog/java/2025-03-08-data-structures
title:  "Array vs Arrays vs ArrayList"
date:   2025-04-20
categories:
  - studylog
  - java
description: >
  Array와 Arrays, ArrayList의 각각의 특징들을 살피고 명확한 차이를 구분
---
* toc
{:toc .large-only}

# Array
* 
* 

# Arrays

## Arrays 특징


# ArrayList
List 인테 페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형 자료구조(데이터를 정해진 순서대로 저장 및 탐색)이다.
## ArrayList 특징
* 연속적인 데이터의 리스트(순차 리스트)
* ArrayList 클래스는 내부적으로 `Object[]` 배열을 이용하여 요소를 저장
* 크기가 고정되어있는 배열과 달리 가변적으로 공간을 늘리거나 줄일 수 있다.
* 그러나 공간이 꽉 찰 때마다 `배열을 copy하는 방식으로 늘려 지연이 발생`하게 된다.
* 데이터를 리스트에 중간에 삽입/삭제 할 경우, 빈 공간이 생기지 않도록 요소들의 위치를 앞뒤로 이동시키기 때문에 삽입/삭제 동작이 느리다.
* 조회를 많이 하는 경우에 사용하는 것이 좋다. 
## ArrayList 선언
```java
import java.util.ArrayList;

ArrayList<Object> list = new ArrayList<>(); // 객체 타입만 사용 가능
```
## ArrayList 메서드
### 요소 추가
* add()
* addAll()
```java
ArryayList<Integer> list = new ArrayList<>();

list.add
```