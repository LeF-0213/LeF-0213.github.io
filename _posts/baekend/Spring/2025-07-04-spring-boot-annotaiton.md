---
layout: post
related_posts:
    - /baekend/spring
title:  "spring boot에서 사용하는 annotaion"
date:   2025-07-04
categories:
  - baekend
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

spring boot Controller
	어떤 요청이 왔을 때 어떤 메소드를 실행시킬 것인지
	짝지어주는 클래스
	ex) GET /study/emp --> aaa라는 메소드를 실행
	
	front가 보내는 요청에는 데이터가 있을 수 있다.
	ex) 삭제하고자 하는 게시글 번호
	    조회하고자 하는 게시글 번호
	    추가하고자 하는 게시글 제목, 내용, 작성자, ...
	
	POST 요청일 경우 데이터를 front가 body에 담아서 준다
	GET 요청일 경우 데이터를 front가 주소에 담아서 준다
		QueryParam
			?변수명=값&변수명=값&변수명=값
		PathVariable
			/값

front가 controller에 요청하면서 body에 담은 값을 전달한다.
이때 controller는 body에 담긴 값을 받기 위해서 DTO를 만든다.

	DTO(Data Transfer Object)
		front가 back으로 혹은 back이 front로 전달할 값들을 모으기 위한 객체

DTO에 들어있는 값을 꺼내서 Entity에 넣어준다.

	Entity 
		back이 DB에 값을 넣거나, DB에서 꺼낸 값을 담을 객체
		Table 컬럼과 변수의 구조가 완전히 동일해야한다.

Mapper interface에 있는 메소드를 사용하여 Entity속에 있는 값을 쿼리로 전달한다.

Mapper xml은 Entity 속에 있는 값을 전달받아서 query에 사용한다.


오류 원인
	spring boot 3버전부터는 mysql 연결하는 방식 변경

	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.3'
	runtimeOnly 'com.mysql-connector-j'

Model 객체
	database 테이블에 값을 전달하거나. database 조회 결과를 담기위한 객체
	테이블의 컬럼 1개가 1개의 값이다.
	즉, 커럶의 갯수만큼 클래스 안의 변수 갯수(인스턴스 변수의 갯수)가 된다.
	따라서 만들 때, 변수 이름도 반드시 연결할 테이블 컬럼의 이름, 타입과 맞춰줘야한다.
	단, 테이블의 컬럼은 _ 표기법으로 되어있고, 이를 java로 받아오면 대문자 표기법으로 고쳐야한다.
	ex) table에는 student_name으로 컬럼이름이 지어져있다면 java에서는 studentName으로 써야한다.

요청의 주체 --> postman(front)

employee 테이블에 사원정보 추가해줘 요청(front)

/study/emp POST --> employee 테이블에 사원정보 추가해줘 요청(front)
				추가할 값들도 전달해야 한다.
				POST요청일 경우에 front는 추가할 값들을 객체에 담아서 전달한다.	
				이때 front가 담아서 전달하는 객체를 body라고 한다.
				front의 요청 안에 body 안에 담아서 전달 (Request Body안에 값들이 있음)

DTO vs Model
	DTO : front와 back이 서로 데이터를 주고받을 때 데이터를 한 번에 담아서 주는 객체
	Model : back과 db가 서로 데이터를 주고받을 때 데이터를 한 번에 담아서 주는 객체
	분리하지 않으면 front가 데이터 주는 방식을 변경했을 때 DB까지 영향을 준다.(DB도 수정해야함)
			db 컬럼을 변경했을 때 front까지 영향을 준다.(front도 수정을 해야함)

	분리가 되면 front가 수정되었을 때는 DTO만 수정해주면 되고, db가 수정되었을 때는 model만 수정하면 된다.

	model은 무조건 데이블의 컬럼 타입과 갯수를 맞추어 똑같이 만든다.
	DTO는 front와 back이 서로 약속한대로 만들어준다.

POST 요청 시 front는 body에 여러개의 값을 담아서 요청할 수 있다.
	이때 body에 담긴 여러개의 값은 back에서 같은 형식으로 정의된 DTO 객체로 받아올 수 있다.
	이때, 매개변수에는 @RequestBody라고 어노테이션을 달아서 알려주어야 한다.

/study/emp3로 POST요청을 한다. 이때 추가해야할 데이터는 front가 body에 담아서 전달한다.
만일 /study/emp3로 POST요청을 하고, body에 {
    "firstName" : "길동",
    "lastName" : "박",
    "email" : "aaa@aa.com",
    "hireDate" : "2024-01-01"
}
이러한 값이 전달되었다면, employee 테이블에다 전달받은 값들을 insert 하시오.

front가 요청 사원정보검색
/study/emp 로 GET요청
	대신 보고싶은 사원번호는 front가 전달한다. 데이터를 PathVariable 혹은 QueryParam 방식을 사용하여 값을 전달한다.

PathVariable
	마치 주소의 일부분처럼 전달하는 방법
	GET /study/emp/5 --> 5번 사원 조회
	GET /study/emp/40 --> 40번 사원 조회

QueryParam
	주소에 변수로 값을 전달하는 방법
	GET /study/emp?employeeId=5 --> 5번 사원 조회
	GET /study/emp?employeeId=40 --> 40번 사원 조회

front가 back에게 전달한 값을 받아오는 방법
1. @RequestBody -> POST요청시에 front가 body에 담아서 전달한 값을 받아올 때 사용
			DTO 객체를 만들어서 받아온다

2. @PathVariable -> 주소에 {변수이름}으로 표현하고, 해당 위치에 입력된 값을
			 @PathVariable 매개변수가 받아온다.
			 이때 name속성에 {} 안에 있는 변수 이름을 써서 매핑해야한다.
			 ex) @GetMapping("/study/emp/{id})
			     public ... getEmp(@PathVariable(name="id") 타입 매개변수명) {
				
			     }

3. @RequestParam --> QueryParam으로도 불리우며 front가 주소에 ? 뒤에 변수명=값 형식으로 전달하는 경우
			  @RequestParam을 통해 매개변수로 받아온다.
			  이때 name속성에 front가 설정한 변수명을 명시해야 한다.
			  ex) front가 "/study/emp?id=500"으로 GET요청을 하면
				@GetMapping("/study/emp")
			      public ... getEmp(@PathVariable(name="id") 타입 매개변수명) {
				
			      }	














