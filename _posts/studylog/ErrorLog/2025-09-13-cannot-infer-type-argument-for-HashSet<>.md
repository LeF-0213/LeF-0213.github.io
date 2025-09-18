---
layout: post
related_posts:
    - /studylog/errorLog
title:  "error: cannot infer type arguments for HashSet<>"
date:   2025-09-13
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
`error: cannot infer type arguments for HashSet<>`

# 문제 원인
이 오류는 **HashSet 생성자에 잘못된 타입의 값을 전달했을 때 발생**한다.   
예를 들어, 원시 타입 배열(`int[]`)을 그대로 전달하거나 제네릭 타입을 명시하지 않으면 Java 컴파일러가 타입을 추론하지 못한다. 

### 문제 상황 예시
```java
int[] arr = {1, 2, 3, 4, 5};

// 오류 발생: 배열을 직접 전달
HashSet<Integer> set = new HashSet<>(arr);
```
위 코드는 `arr`이 `int[]` 타입이므로 `HashSet`의 생성자가 요구하는 `Collection` 타입과 맞지 않아 에러가 발생한 것이다.

# 문제 해결
원시 타입 배열을 바로 사용할 수 없으므로, **List 형태로 변환한 후 HashSet에 전달해야 한다.**
### Arrays.asList() 사용
```python

```

```python

```

```python

```
1. Arrays.asList() 사용
Integer[] arr = {1, 2, 3, 4, 5};
HashSet<Integer> set = new HashSet<>(Arrays.asList(arr));
System.out.println(set); // [1, 2, 3, 4, 5]

2. List.of() 사용 (Java 9 이상)
HashSet<Integer> set = new HashSet<>(List.of(1, 2, 3, 4, 5));
System.out.println(set); // [1, 2, 3, 4, 5]

3. IntStream 활용 (원시 타입 배열인 경우)
int[] arr = {1, 2, 3, 4, 5};
HashSet<Integer> set = new HashSet<>(
    IntStream.of(arr).boxed().toList()
);
System.out.println(set); // [1, 2, 3, 4, 5]

✅ 정리

HashSet의 생성자는 Collection 타입을 받음 → 원시 타입 배열은 직접 전달 불가

Arrays.asList(), List.of(), IntStream.boxed() 등을 활용해 배열을 컬렉션으로 변환해야 함

제네릭 타입(HashSet<Integer>)을 반드시 명시해야 컴파일 오류를 방지할 수 있음