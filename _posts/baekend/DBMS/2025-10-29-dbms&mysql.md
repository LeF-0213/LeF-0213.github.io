---
layout: post
related_posts:
    - /baekend/dbms
title:  "DBMS와 Mysql"
date:   2025-10-29
categories:
  - baekend
  - dbms
description: >
  
---
* toc
{:toc .large-only}

![DBMS](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*sS0ry8CtQ7WwFeE3.png)

# 데이터(Data)란?
* **가공되지 않은 사실이나 값의 집합**
* 측정을 통해 얻은 사실, 수치, 문자, 기호, 이미지 등

# 데이터베이스(DataBase, DB)란?
* 여러 사람이 동시에 효율적으로 데이터를 저장, 검색, 수정, 삭제할 수 있도록 체계적으로 관리하는 **데이터의 집합**

# DBMS(DataBase Management System. 데이터베이스 관리 시스템)란?
* 데이터베이스를 관리할 수 있는 기술적인 소프트웨어
* [DB-Engines](https://db-engines.com/en/ranking)

# 관계형 데이터베이스(Relational Database)
* 데이터를 **행(Row)과 열(Column)로 구성된 표형태(테이블)**로 저장하고, 
* **테이블 간의 관계**를 통해 데이터를 연결하여 관리하는 데이터베이스

# SQL(Structured Query Lanuage)
* 관계형 데이터베이스에서 데이터를 정의하고 조작하며 제어하기 위한 표준 언어

# NoSQL(Not Only SQL)
* 전통적인 관계형 데이터베이스의 한계를 보완하기 위해 등장한 **비관계형 데이터베이스**
* 문서(Document), 키-값(Key-value), 컬럼(Colum), 그래프(Graph) 등 다양한 형태로 데이터를 저장하여 더 유연하고 확장성 있게 데이터를 관리

### MySQL 8.0.44 설치 경로
* [[mac]mysql community server download 8.0.44](https://dev.mysql.com/downloads/mysql/)
* [[mac]mysql workbench server download 8.0.44](https://dev.mysql.com/downloads/workbench/)

# [MySQL](https://wikidocs.net/226173)

## 개념 정리
### 테이블
데이터를 행과 열(컬럼, 필드, 어트리뷰트)로 스키마에 따라 저장할 수 있는 구조

```sql
create table 테이블명(
  컬럼명1 데이터타입 제약조건,
  컬럼명2 데이터타입 제약조건,
  ...
)
```

### 스키마
데이터 베이스의 구조와 제약 조건에 관한 명세를 기술한 집합

### 데이터 타입
* 정수: INT, BIGINT
* 실수: FLOAT, DOUBLE, DECIMAL
* 문자형: CHAR, VARCHAR(최대 65535byte), BINARY, VARBINARY, TEXT
* 날짜형: DATE, TIME, DATETIME, TIMESTAMP

## 제약 조건
데이터의 무결성을 지키기 위해 데이터를 입력받을 때 실행 되는 검사 규칙

> **NOT NULL**: null 값을 허용하지 않음       
**UNIQUE**: 중복값을 허용하지 않음. null 값은 허용        
**DEFAULT**: null값을 삽입할 때 기본이 되는 값을 설정           
**기본키(PRIMARY KEY, PK)**:
  * null 값을 허용하지 않음       
  * 중복값을 허용하지 않음        
  * 테이블에 단 하나의 컬럼에만 설정(MySQL에서만)       
  * 인덱싱 설정         
  * 참조키(FOREIGN KEY)와 쌍으로 연결      
>     
**참조키(FOREIGN KEY, FK)**: 기본키와 쌍으로 연결

## 명령어
```sql
SHOW DATABASE;

CREATE DATABASE [데이터베이스명];

USE [데이터베이스명];

CREATE TABEL ();

DROP TABLE [테이블명];


```