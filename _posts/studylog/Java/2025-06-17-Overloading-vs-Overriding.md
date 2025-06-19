---
layout: post
related_posts:
    - /studylog/java
title:  "오버로딩(+ 생성자) VS 오버라이딩(업캐스팅 VS 다운캐스팅)"
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

업캐스팅
	자식타입을 부모타입으로
	자식객체를 부모타입의 변수에 대입 --> 업캐스팅되어 대입되는 것
    공통의 타입으로 관리해야할 때 업캐스팅을 할 수밖에 없다.

다운캐스팅
	부모타입을 자식타입으로
	부모타입으로 업캐스팅 된 대상을 다시 자식타입으로 바꾸는 것
	업캐스팅하면 자식타입에 새롭게 만든 멤버는 사용을 못하니
	그 멤버를 사용하고 싶다면 다시 자식타입으로 다운캐스팅해서 사용한다.

instanceof 연산자
	1. 피연산자의 갯수, 타입
		두개, 앞에는 객체(instance) 뒤에는 클래스이름
	2. 동작
		앞에있는 인스턴스가(객체가) 뒤에있는 클래스타입의 인스턴스인지 확인해준다.
	3. 결과 값
		맞으면 true, 아니면 false

	ex) ec1 instanceof ElecCar
	    ec1 instanceof HybridCar