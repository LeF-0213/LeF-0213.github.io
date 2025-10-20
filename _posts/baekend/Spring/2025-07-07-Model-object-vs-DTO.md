---
layout: post
related_posts:
    - /baekend/spring
title:  "spring Model 객체 VS DTO"
date:   2025-07-05
categories:
  - baekend
  - spring
description: >
  Model 객체와 DTO에 대한 개념과 두 객체의 차이점에 대한 이해
---
* toc
{:toc .large-only}

프론트에서 back은 DTO로 연결, back에서 database는 Model 객체로 연결

Model 객체
	database 테이블에 값을 전달하거나. database 조회 결과를 담기위한 객체
	테이블의 컬럼 1개가 1개의 값이다.
	즉, 커럶의 갯수만큼 클래스 안의 변수 갯수(인스턴스 변수의 갯수)가 된다.
	따라서 만들 때, 변수 이름도 반드시 연결할 테이블 컬럼의 이름, 타입과 맞춰줘야한다.
	단, 테이블의 컬럼은 _ 표기법으로 되어있고, 이를 java로 받아오면 대문자 표기법으로 고쳐야한다.
	ex) table에는 student_name으로 컬럼이름이 지어져있다면 java에서는 studentName으로 써야한다.

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
