---
layout: post
related_posts:
    - /studylog/dbms
title:  "spring boot의 MVC 패턴과 디렉터리 구조(계층형 vs 도메인형)"
date:   2025-07-05
categories:
  - studylog
  - spring
description: >
  spring boot의 MVC 패턴과 각각의 용어에 대한 설명하고, 계층형구조와 도메인형 구조에 따른 디렉터리 구조의 이해
---
* toc
{:toc .large-only}

# MVC 패턴이란?

## Model

## View

## Controller

spring boot Controller
    매핑된 메소드를 실행
	이 메소드 내부에서 Mapper Interface의 메소드를 실행하면
	Mapper xml을 찾아가서 동일한 id를 찾고, SQL을 실행한다.
	
	SQL실행결과가 Mapper interface 실행 결과가 되고,
	Mapper interface 메소드 실행 결과가 Controller 메소드 싫애 결과가 된다.
	Controller 메소드 실행 결과는 프론트에게 보내지는 응답이 된다.

