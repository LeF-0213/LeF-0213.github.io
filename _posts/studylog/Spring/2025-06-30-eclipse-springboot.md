---
layout: post
related_posts:
    - /studylog/spring
title:  "eclipse에서의 Springboot 설치와 설정"
date:   2025-06-30
categories:
  - studylog
  - spring
description: >
  eclipse에서의 Springboot 설치와 설정
---
* toc
{:toc .large-only}

# SpringBoot란?
Java로 개발된 웹 애플리케이션을 만들기 쉽게 도와주는 프레임워크로, 개발자가 빠르게 애플리케이션을 개발할 수 있도록 돕는다.
**특징**
* 복잡한 설정 최소화
* 기본적인 웹 서버 자동 설정
* intellij IDE, eclipse IDE를 사용한다

### spring boot Eclipse 버전 설치 방법
1. google에 spring boot eclipse 검색
2. Spring 공식 홈페이지(Spring | Tools)에서 본인의 운영체제에 맞는 버전 다운로드
3. 다운로드된 압축 파일 압축 해제
4. `실행 시 workspace 설정 주의`(eclipse는 항상 경로 설정 주의!)

## spring boot 프로젝트 만들기(MacOS)
1. alt command n -> spring starter project를 만든다.
2. Java 버전을 선택한다(내 컴퓨터에 깔려있는 버전)
3. type은 Gradle - Groovy선택(MAVEN은 옛날 버전)
4. Next -> Spring Web을 선택한 후 만들기

## spring boot starter project란?
	기본적으로 필요한 폴더들이 들어있는 채로 만들 수 있는 kit
	build.gradle -> 프로젝트와 관련된 설정을 하는 파일
			   ex) 프로젝트에서 사용하는 자바 버전 /
				 프로젝트에서 사용하는 라이브러리
				 프로젝트의 이름은 뭔지 / 소유자 / 버전...
	
	src/main/resource 안에 있는 application.properties 파일
		애플리케이션의 속성정보를 저장하는 파일
			ex) 이 앱의 이름
			    이 앱에서 사용하는 db는 뭔지
			    이 앱에서 사용하는 모드는 뭔지(개발모드, 사용모드)

src/main/java 폴더
	해당 폴더 안에 자바 코드를 작성하게 된다.

	기본으로 만들어져있는 NeflixApplication.java는 main 메소드를 갖고 있다.
	application 실행을 클릭하면 해당 main메소드가 실행되고, 오류가 나지 않는 이상 꺼지지 않는다.

  encoding 설정
	프로젝트 오른쪽 클릭 -> Properties -> Resource
	-> encoding을 UTF-8로 설정 -> Apply & Close

port 번호 설정
	port는 프로그램이 실행중인 위치
	기본은 8080에서 실행이 된다.
	이때, 8080이 이미 사용중일 수 있기 때문에 적절한 port로 직접 설정해줘야 한다.
	application.properties 파일에 
		server.port=번호
	를 입력하여 설정한다.

REST API
서로 다른 시스템(클라이언트와 서버)이 데이터를 주고 받을 때 사용하는 규칙
주소의 형태로 식별하자
naver.com/movie --> 영화와 관련된 기능이 실행
                영화목록을 조회
                영화를 추가
                영화를 삭제
                영화를 수정
method(방식)으로 기능을 구분
    GET		조회
    POST		추가
    PUT 		수정
    DELETE		삭제

    GET요청		naver.com/movie --> 영화조회
    POST요청	naver.com/movie --> 영화추가

Controller 클래스
	요청과 실행시킬 메소드를 짝 지어주는 클래스
	@Controller라는 어노테이션을 통해서 알려줘야 한다.
    ```java
        import org.springframework.stereotype.Controller;

        @Controller
        public class Controller {
        
        }
    ```

	메소드 위에서 @GetMapping, @PostMapping, @PutMapping, @DeleteMapping등의
	어노테이션을 통해 방식을 지정하고, ()안에 주소를 지정한다,
	ex)
		@GetMapping("/abcd/efg")
		public void qqq() {
			...생략...
		}

		--> qqq메소드는 /abcd/efg로 GET 요청이 왔을 때 실행된다.

전통적인 방식
	Server Side Rendering
		Get 요청이 오면 알맞은 html화면 코드를 그려서 응답해주고,
		응답을 받은 클라이언트는 그 코드를 그대로 화면에 보여준다.
		장점) 검색 최적화
		단점) 속도가 상대적으로 느리다(CSR에 비해서)

새로운 방식
	서버는 화면을 그리지 말고 화면에 필요한 데이터만 줘라
	데이터를 받은 클라이언트가 화면을 그린다. Client Side Rendering
	Client는 화면을 그릴 때 바뀐 부분만 그릴 수 있음
	--> 장점) 최초 렌더링이 된 상태에서는 상대적으로 빠르고, 유연한 느낌의 상호작용 가능
	    단점) 검색 최적화 등을 조금 더 신경써야한다.

http success codes(`https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status`)

```java
    @GetMapping("/study2/movie")
    public ResponseEntity<String> f11() {
        System.out.println("LeF");
		// ResponseEntity<String> res = new ResponseEntity<String>("LeFLeF", HttpStatusCode.valueOf(200));
        
        String contents = "<h1>안녕하세요</h1>"
                + "<p>오늘은 html, spring boot를 배우고 있어요.</p>"
                + "<input />";
        
        return ResponseEntity.status(200).body(contents);
    }
```

```java
    @GetMapping("/study/movie")
	public ResponseEntity<String> f1() {
		System.out.println("LeF");
		// ResponseEntity<String> res = new ResponseEntity<String>("LeFLeF", HttpStatusCode.valueOf(200));
        // return res;
		return ResponseEntity.status(200).body("LeFLeF");
	}
```

html
	태그로 이루어진 문서
	html문서는 브라우저(크롬, 엣지, 사파리)가 연다.
	브라우저가 html문서를 열때 html 태그를 보고 구조를 분석하여 열어준다.

	규칙
	태그로 이루어져 있다.
	태그는 <> 형태이다.
	ex) <p> --> p태그
	    <h1> --> h1태그
	태그 안에는 속성이 존재한다(태그에 대한 부가적인 정보)
		속성은 속성이름="값" 형식으로 이루어져있는 것이 일반적이다.
		속성에 사용할 수 있는 값이 둘 중 하나인 경우(True False)에는
		속성값을 생략하기도 한다.(아래 예시에서 required속성엔 True값이 적용됨)
		여러개의 속성은 띄어쓰기로 구분된다.
	ex) <input placeholder="id 입력" type="text" required>