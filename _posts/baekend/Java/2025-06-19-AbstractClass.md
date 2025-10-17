---
layout: post
related_posts:
    - /studylog/java
title:  "추상클래스 vs 인터페이스 vs 구현체"
date:   2025-06-19
categories:
  - studylog
  - java
description: >
  추상클래스와 인터페이스 개념과 사용목적에 대해 이해, 구현체와의 차이
---
* toc
{:toc .large-only}


# 추상클래스란?
클래스는 클래스인데, 추상 메소드를 포함하고 있는 클래스

## 추상메소드란?
메소드는 메소드인데, 선언만 되어있고, 구현(`{}`)은 되어있지 않은 메소드
### 예시
```java
    public abstract void method();
```

# 인터페이스란?
abstract 클래스를 만들어서 일반메소드 포한하지 않고, 추상메소드만 들어있는 추상클래스를 제공하는 경우가 너무 많아져서,
아예 일반 메소드는 포함할 수 없는 Interface라는 문법을 만들어서 제공하기 시작했다.
추상클래스는 추상클래스인데, 추상메소드만 들어있는 추상클래스
```java
    public interface Animal {
	// 어차피 추상메소드만 들어있을 수 있기 때문에, abstract 키워드 생략 가능
	public void walk();
	public void feed();
	public void rest();
    }
```

```java
    public class Tiger implements Animal {
        @Override
        public void walk() {
            // TODO Auto-generated method stub
            
        }

        @Override
        public void feed() {
            // TODO Auto-generated method stub
            
        }

        @Override
        public void rest() {
            // TODO Auto-generated method stub
            
        }
	
    }
```

```java
    public class AbstractTest {
        public static void main(String[] args) {
            // 추상클래스는 객체를 만들 수 없다.
            // Test a = new Test();
            
            // 인터페이스도 객체를 만들 수 없다.
    //		Animal a = new Animal();
            Tiger a = new Tiger();
            
            // 업캐스팅
            Animal b = new Tiger();
            b.walk();
	    }
    }
```

### 활용 예시
List 인터페이스를 이용해 ArrayList와 LinkedList에 상속
```java

```

# 구현체란?
