---
layout: post
related_posts:
    - /studylog/java
title:  "오버로딩(+ 생성자) VS 오버라이딩(업캐스팅 VS 다운캐스팅, Wrapper 클래스)"
date:   2025-06-17
categories:
  - studylog
  - java
description: >
  오버로딩과 오버라이딩의 차이를 생성자, 업캐스팅과 다운캐스팅의 활용예시를 통해 이해
---
* toc
{:toc .large-only}

```java
	// @ : annotation 주석 -> 컴퓨터에게 설명하기 위한 주석
	// 
	super.show();

	// 자동으로 생성되는 기본생성자
	public ElecCar() {
		super(); // 부모생성자를 의미, 부모의 멤버들을 받아온다. 생략가능.
	}

	// 다운캐스팅
	// 자식타입은 부모타입보다 크기가 크기 때문에,
	// 부모를 자식으로 바꾸게 되면 빈 공간이 생겨서 불가능하다.
	// 부모타입으로 업캐스팅 된 대상을 다시 자식타입으로 바꾸는 것
	// EleCar qqq = (ElecCar)(new Car(1000, "김영희")); // EleCar cannot be resolved to a type
	ElecCar down = (ElecCar)tmp;
	down.openRoof();
	down.mode = "normal";

	// 업캐스팅이 필요한 경우, 하나의 공통 타입으로 관리
	Car[] cars = {(Car)ec1, (Car)ec2, (Car)hc1, (Car)hc2}; 
	// 다운캐스팅 활용
	((ElecCar)cars[0]).openRoof(); 

	public class InheritTest2 {
		public static void main(String[] args) {
			ElecCar ec1 = new ElecCar(1000, "홍길동", "sports");
			ElecCar ec2 = new ElecCar(2000, "김철수", "normal");
			HybridCar hc1 = new HybridCar(3000, "김영희", "전기");
			HybridCar hc2 = new HybridCar(3000, "고길동", "가솔린");
		
			InheritTest2.tmp(ec1);
			InheritTest2.tmp(hc1);
			
			// 하나의 공통 타입으로 관리
			Car[] cars = {(Car)ec1, (Car)ec2, (Car)hc1, (Car)hc2};
			((ElecCar)cars[0]).openRoof();
		}

		// qqq에는 ElecCar타입 혹은 HybridCar타입
		public static void tmp(Car car) {
			// car에는 EleCar 혹은 HybridCar 타입 객체가 업캐스팅되어 들어온다.
			if(car instanceof ElecCar) {
				((ElecCar)car).openRoof();
			} else {
				((HybridCar)car).changeStatus();
			}
		}
	}
```

## 업캐스팅(자식타입을 부모타입으로)
자식객체를 부모타입의 변수에 대입 --> `업캐스팅`되어 대입되는 것  
공통의 타입으로 관리해야할 때 업캐스팅을 할 수밖에 없다.

## 다운캐스팅(부모타입을 자식타입으로)
**부모타입으로 업캐스팅 된 대상을 다시 자식타입으로 바꾸는 것**  
`업캐스팅`하면 자식타입에 새롭게 만든 멤버는 사용을 못하니,  
그 멤버를 사용하고 싶다면 다시 자식타입으로 `다운캐스팅`해서 사용한다.

## instanceof 연산자
1. **피연산자의 갯수, 타입**  
	두개, 앞에는 객체(instance) 뒤에는 클래스이름
2. **동작**  
	앞에있는 인스턴스가(객체가) 뒤에있는 클래스타입의 인스턴스인지 확인해준다.
3. **결과 값**  
	맞으면 true, 아니면 false  
  ex)   
  - ec1 instanceof ElecCar
  - ec1 instanceof HybridCar
```java
public static void main(String[] args) {
	int a = 10;
	int b = 10;
	Integer c = new Integer(10);
	Integer d = new Integer(10);
	
	// c라는 객체가 10으로 unboxing 된 후에 비교가 되고 있다.
	System.out.println(a == c);
	
	// a --> Integer타입으로 boxing --> Object 타입으로 업캐스팅
	System.out.println(c.equals(a));
	
	// == 연산자는 객체와 객체를 비교할 때 
	// 서로 물리적으로 동일한 위치에 있는 객체인지 비교한다.
	System.out.println(c == d); // c와 d는 물리적으로 다른 객체이기 때문에 false
	
	// 논리적으로 10을 사용 못하니, 10이 들어있는 객체를 만든 것이고,
	// 논리적으로 10이 들어있는 두 객체는 동일한지 검사하고 싶을 때는 equals 메소드를 사용한다.
	System.out.println(c.equals(d));
}
```

## Wrapper 클래스
기본자료형이 아니기 때문에 **클래스형 객체**를 사용해야만 하는 경우가 있다.  
따라서 **기본자료형에 해당하는 Wrapper클래스**를 JAVA에서는 제공을 한다.  
기본 타입(클래스 형 타입을 제외한 타입, int double char boolean)이 아닌,  
**반드시 클래스형 타입을 사용해야 하는 경우 존재**한다  

**기본타입을 감싸는 Wrapper 클래스**  
- int --> Integer
- double --> Double
- char --> Character

## boxing 
**기본자료형 -> Wrapper 클래스 타입으로 변환** 

- 생성자를 사용하면 기본자료형 값이 들어있는 객체가 만들어진다.  
	ex) new Integer(10);   
	--> 10이 요소로 들어있는 Integer타입 객체  
- 보통 생성자를 사용하는 것이 생략된 형태로 `auto boxing`이 일어난다.  
	ex) Integer a = 10;

## unboxing
**Wrapper 클래스타입 -> 기본자료형으로 변환**  

- 객체안에 있는 값을 return하는 메소드를 사용한다.  
	ex) Integer객체.intValue(); --> int값이 return됨
- 보통 메소드를 사용하는 것이 생략된 형태로 `auto unboxing`이 일어난다  
	ex) int b = a;