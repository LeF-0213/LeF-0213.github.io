---
layout: post
related_posts:
    - /studylog/dbms
title:  "spring boot에서 사용하는 annotaion"
date:   2025-06-25
categories:
  - studylog
  - spring
description: >
  spring boot에서 사용하는 어노테이션
---
* toc
{:toc .large-only}

## @Mapper
이 인터페이스에 있는 메소드는 src/main/resources/mappers 안에 mapper.xml 파일에서 정보를 찾아오게 한다.

## @GetMapping

## @PostMapping

## @Autowired
알아서 변수에다가 알맞는 타입의 객체를 생성시킨 뒤 값을 넣게한다. 알아서 db연결 쿼리 실행 db닫기까지 코드를 만들어준다. 
의존성 주입
개발자가 로직에 집중할 수 있게 프레임워크를 사용하는 이유

## @RequestBody