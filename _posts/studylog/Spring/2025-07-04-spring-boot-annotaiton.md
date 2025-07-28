---
layout: post
related_posts:
    - /studylog/spring
title:  "spring boot에서 사용하는 annotaion"
date:   2025-07-04
categories:
  - studylog
  - spring
  - db
description: >
  spring boot에서 사용하는 어노테이션
---
* toc
{:toc .large-only}

## @Mapper
이 인터페이스에 있는 메소드는 src/main/resources/mappers 안에 mapper.xml 파일에서 정보를 찾아오게 한다.
mapper interface는 메소드와 쿼리(mapper xml)를 짝지어주는 인터페이스이다. 
```java
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface EmployeeMapper {
	public void insertNewEmployee(Employee e);
}
```

## @Controller

## @GetMapping

## @PostMapping

## @Autowired
알아서 변수에다가 알맞는 타입의 객체를 생성시킨 뒤 값을 넣게한다. 알아서 db연결 쿼리 실행 db닫기까지 코드를 만들어준다. 
spring boot가 직접 의존성을 주입한다(객체를 만들어줌)
개발자가 로직에 집중할 수 있게 프레임워크를 사용하는 이유

## @RequestBody
POST요청시에 front가 body에 담아서 전달한 값을 받아올 때 사용
DTO 객체를 만들어서 받아온다

## @PathVariable
**front가 전달하고자 하는 값이 UNIQUE한 값일 때, PK알때 주로 사용**
주소에 {변수이름}으로 표현하고, 해당 위치에 입력된 값을
@PathVariable 매개변수가 받아온다.
이때 name속성에 {} 안에 있는 변수 이름을 써서 매핑해야한다.
ex) @GetMapping("/study/emp/{id})
    public ... getEmp(@PathVariable(name="id") 타입 매개변수명) {

    }
```java
    @GetMapping("/study/emp/{aaa}")
	public ResponseEntity<String> emp(@PathVariable(name = "aaa")  int dddd) {
		System.out,println(dddd); // aaa = dddd
		return ResponseEntity.status(200).body("okay");
	}
```
postman 요청 예시 : `localhost:8000/study/emp/5`
dddd 값 : 5


## @RequestParam
```java
    @GetMapping("/study3/param")
	public ResponseEntity<String> f2(
			@RequestParam(name = "startAge", defaultValue = "0") Integer from,
			@RequestParam(name = "endAge", required = false) Integer to
		) {
		System.out.println("from에는 : " + from);
		System.out.println("to에는 : " + to);
		return ResponseEntity.status(200).body("성공");
	}
```
postman 요청 예시 : `localhost:8000/study/emp?employeeId=50&qqq=홍길동`
출력 결과 : 
a에 들어있는 값 : 50
b에 들어있는 값 : 홍길동

## @Param