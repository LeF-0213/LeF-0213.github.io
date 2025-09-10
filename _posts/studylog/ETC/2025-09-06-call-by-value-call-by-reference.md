---
layout: post
related_posts:
    - /studylog/etc
title:  "Call by Value vs Call by Reference(얕은 복사, 깊은 복사)"
date:   2025-09-06
categories:
  - studylog
  - etc
description: >
  Call by Value와 Call by Reference, 얕은 복사와 깊은 복사, 값 타입과 참조 타입
---
* toc
{:toc .large-only}

# Call by Value vs Call by Reference
## Call by Value란?
함수가 인수로 전달받은 값을 복사하여 처리하는 방식으로, 함수 내에서 **값을 변경해서 원본 값은 변경되지 않는다.** 즉, **불변성**을 유지하는데 용이하다.
* 장점 : 복사하여 처리하기 때문에 안전하다. 원래의 값이 보존이 된다,
* 단점 : 복사를 하기 때문에 메모리 사용량이 늘어난다.
  
## Call by Refrence란?
인자의 값이 매개변수에 복사되는 점은 동일하지만, 복사되는 값이 데이터의 주소 값이라는 차이점이 존재한다. 이 방식에서 **함수 내에서 인자로 전달된 변수의 값을 변경하면, 호출한 쪽에서도 해당 변수의 값이 변경된다.**
* 장점 : 직접 참조하기에 빠르다
* 단점 : 직접 참조를 하기에 원래 값이 영향을 받는다.

### Call by Refrence의 단점 보완
Call by Reference는 참조를 직접 다루기 때문에, 
**함수 내부에서 값이 변경되면 원본 객체도 함께 변경**되는 문제가 있다.
이를 보완하기 위해 **얕은 복사(Shallow Copy)**와 **깊은 복사(Deep Copy)** 개념이 등장한다.

## 얕은 복사(Shallow Copy) vs 깊은 복사(Deep Copy)
### 얕은 복사(Shallow Copy)
* 객체의 **주소 값만 복사**하는 방식이다.
* 따라서 복사본과 원본이 같은 객체를 가리키게 되며, 한쪽에서 수정하면 다른쪽에도 반영된다.
* **Call by Reference의 단점(원본 변경 가능성)을 그대로 가진다.**
* 하지만 속도가 빠르고 메모리 사용이 적다는 장점이 있다.
```java
public class Main {
  public static void main(String[] args) {
    int[] arr1 = {1, 2, 3};
    int[] arr2 = arr1;  // 얕은 복사

    arr2[0] = 99;
    System.out.println(arr1[0]); // 99 (원본도 함께 변경)
  }
}
```

### 깊은 복사(Deep Copy)
* 객체가 가진 데이터를 **새로운 메모리 공간에 복사**한다.
* 복사본과 원본이 서로 다른 객체를 바라보기 때문에, 한쪽의 변경이 다른 쪽에 영향을 주지 않는다.
* **Call by Reference의 단점을 보완**하는 방식이다.
* 단, 메모리 사용량이 늘어나고 성능이 떨어질 수 있다.
```java
import java.util.Arrays;

public class Main {
  public static void main(String[] args) {
    int[] arr1 = {1, 2, 3};
    int[] arr2 = Arrays.copyOf(arr1, arr1.length); // 깊은 복사

    arr2[0] = 99;
    System.out.println(arr1[0]); // 1 (원본은 변경되지 않음)
  }
}
```
```java
public class Main {
  public static void main(String[] args) {
    int[] original = {1, 2, 3};
    int[] copy = new int[original.length];

    for (int i = 0; i < original.length; i++) {
        copy[i] = original[i];
    }
  }
}
```

## 값 타입(Value Type) vs 참조 타입(Reference Type)
### 값 타입(Value Type)
* 기본 자료형(Primitive type): `int`, `double`, `boolean`, `char` 등
* 메모리에 **실제 값 자체**가 저장됨
* 함수에 전달될 때 값이 복사되므로, 온본 데이터에는 영향을 주지 않음

```java
public class Main {
  public static void main(String[] args) {
    int a = 10;
    int b = a; // 값 복사
    b = 20;

    System.out.println(a); // 10 (원본은 그대로)
    System.out.println(b); // 20
  }
}
```

### 참조 타입(Reference Type)
* 객체(Object), 배열(Array), 컬렉션(Collection) emd
* 메모리에 **주소 값(참조)**이 저장됨
* 함수에 전달될 때 주소 값이 복사되므로, 같은 객체를 바라보게 됨
* 따라서 한쪽에서 값을 바꾸면 다른 쪽에도 영향을 미침
```java
public class Main {
  public static void main(String[] args) {
    int[] arr1 = {1, 2, 3};
    int[] arr2 = arr1; // 주소 값 복사

    arr2[0] = 99;

    System.out.println(arr1[0]); // 99 (원본도 변경)
    System.out.println(arr2[0]); // 99
  }
}
```

