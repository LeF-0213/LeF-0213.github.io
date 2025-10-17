---
layout: post
related_posts:
    - /studylog/java
title:  "Exception 타입 예외처리 방법 try catch구문과 thorws 비교"
date:   2025-06-25
categories:
  - studylog
  - java
description: >
  Generic의 개념과 List, Set, Map에서의 활용
---
* toc
{:toc .large-only}

```java
public class ThrowsTest {
	public static void rest() 
			throws InterruptedException,
			NumberFormatException,
			ArrayIndexOutOfBoundsException,
			ArithmeticException {
		System.out.println("시작");
		Thread.sleep(3000);
		System.out.println("끝");
	}
	
	public static void main(String[] args) throws NumberFormatException, ArrayIndexOutOfBoundsException, ArithmeticException, InterruptedException {
		ThrowsTest.rest();
	}
}
```

**main**에 `throws`를 사용해도 예외처리 불가(**main은 컴퓨터가 실행하는 것이라 throws를 써주어도 예외가 실행될 수 없음**)
```java
try {
	main()
} catch(Exception e) {
	// 처리 불가
}
```


## try catch 구문
java에서 예외를 처리할 때 사용하는 문법
java에서는 예외가 Exception이라는 클래스로 만들어져있다.
Exception 타입의 객체가 생성되고, 이 객체가 throw되면 오류가 발생하는 원리

Exception
	IOException
	InterruptedException
	...
	RuntimeException
		IndexOutOfBoundsException
		ArthmethicException
		...

오류 발생 시키는 명령어
	throw 오류객체;
	
	해당 코드가 실행되면 오류객체 형태의 오류가 실제로 발생된다.

Exception 타입 오류 발생시킬 때 vs IndexOutOfBoundsException 타입 오류 발생시킬 때

Exception 타입을 바로 상속받아 만들어진 오류
	컴파일 오류 --> 실행시키기 전에 빨간 밑줄로 오류를 표시해주는 오류
			  반드시 오류를 처리해야지만 코드를 작성하도록 강제하고 싶을 때 사용

RuntimeException 타입을 상속받아 만들어진 오류
	런타임 오류 --> 실행 중에 오류가 발생된다.
			  버그의 주된 원인



try catch
	thorw된 Exception 타입의 객체를 잡아내는 구문

	try{
		오류가 발생할 수 있는 코드;
		오류가 발생할 수 있는 코드;
		오류가 발생할 수 있는 코드;
	}catch(타입1 변수명){
		타입1 유형의 오류가 발생하면 실행할 코드;
	}catch(타입2 변수명){
		타입2 유형의 오류가 발생하면 실행할 코드;
	}....{

	}catch(Exception 변수명) {
		Exception 타입의 오류가 발생하면 실행할 코드;
		(모든 오류는 Exception이 부모이기 때문에, 어떤 오류가 발생하더라도 해당영역이 실행된다.
		upcasting되어 변수에 자식 객체가 들어감)
	}finally{
		강제종료 되기 전에 한 번 실행되어야 하는 코드;
	}



throws 명령어
	예외 처리를 떠넘기는 문법
	메소드를 사용하는 사람이 자유롭게 예외처리할 수 있도록 자율성을 보장
	또한 메소드를 실행하면 어떤 오류가 발생할 수 있는지에 대한 정보도 제공.

출력
	println();
입력
	nextLine();

파일 출력
	콘솔창에 출력 --> println 대신 파일에 출력하겠다.

	1. 파일객체만들기(문자열로 경로를 설정한다)
		경로지정 방법
			상대경로
				현재 실행하는 프로젝트 폴더가 기준
				. 이 의미하는 것은 현재위치 를 의미
				/ 가 의미하는 것은 ~안에 있는 을 의미
				.. 이 의미하는 것은 상위폴더 를 의미
				또한 맨 앞에 ./ 는 생략이 가능하다.
			절대경로
				C 혹은 D드라이브부터 시작하는 fullpath

	2. 스트림 열기
		FileWriter 객체를 만든다.
		이때 FileWriter 생성자에는 파일 이름을 인자로 넘겨준다.
		스트림이란? 데이터를 전송하기 위해 필요한 객체
	3. 버퍼만들기
		BufferedWriter 객체를 만든다.
		이때 BufferedWriter 생성자에는 스트림 객체를 넘겨준다.
	4. 파일에 출력하기
		write 메소드 사용, 이때 엔터는 포함되어있지 않다.
	5. 버퍼 닫기
		close 메소드 사용.
	6. 스트림 닫기
		close 메소드 사용.

파일(로부터) 입력
	1. 파일 준비
		문자열로 파일 경로 설정(파일이 없으면 오류가 발생한다)
	2. 스트림 객체 만들기
		FileReader 객체 만들기, 생성자의 인자로는 파일 경로를 작성한다.
	3. 버퍼 만들기
		BufferedReader 객체 만들기, 생성자의 인자로는 FileReader 객체 전달
	4. 입력 받기
		readLine 메소드를 사용한다.
		더 이상 읽을 줄이 없다면 null 값이 결과로 return 된다.
	5. 버퍼 닫기
		close 메소드
	6. 스트림 닫기
		close 메소드



























