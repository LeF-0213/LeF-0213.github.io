---
layout: post
related_posts:
    - /baekend/dbms
title:  "DBMS와 MySQL"
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

### 스키마
데이터 베이스의 구조와 제약 조건에 관한 명세를 기술한 집합

```sql
create table 테이블명(
  컬럼명1 데이터타입 제약조건,
  컬럼명2 데이터타입 제약조건,
  ...
)
-- 참조: https://wikidocs.net/226173
```

### 데이터 타입

| **분류** | **데이터 타입** | **설명** |
|:----------| ------------- | -------------------------- |
| **정수(Integer)** | INT, BIGINT | 정수값 저장 |
| **실수(Floating / Decimal)** | FLOAT, DOUBLE | 부동소수점 실수 저장 |
|   | DECIMAL | 고정 소수점(정확한 소수 계산 필요할 때 사용) |
| **문자형(String)** | CHAR | 고정 길이 문자열 |
|   | VARCHAR | 가변 길이 문자열 (최대 65,535 byte) |
|   | BINARY, VARBINARY | 이진(binary) 데이터 저장 |
|   | TEXT | 긴 문자열 저장 |
| **날짜형(Date / Time)** | DATE | 날짜만 저장 |
|   | TIME | 시간만 저장 |
|   | DATETIME | 날짜 + 시간 저장 |
|   | TIMESTAMP | 타임존 기반의 날짜 + 시간 


## 제약 조건
데이터의 무결성을 지키기 위해 데이터를 입력받을 때 실행 되는 검사 규칙

| **제약 조건** | **설명** |
|:-------------:|----------|
| **NOT NULL** | `NULL` 값을 허용하지 않음 |
| **UNIQUE** | 중복값을 허용하지 않지만 `NULL` 값은 여러 개 허용 |
| **DEFAULT** | 값을 지정하지 않을 경우 자동으로 적용되는 기본값 설정 |
| **PRIMARY KEY (기본키, PK)** | - `NULL` 불가<br>- 중복 불가<br>- 한 테이블에 하나만 설정 가능(MySQL 기준)<br>- 자동 인덱스 생성<br>- 외래키(FK)에서 참조하는 대상 |
| **FOREIGN KEY (참조키, FK)** | - 다른 테이블의 **PRIMARY KEY** 또는 **UNIQUE KEY** 를 참조<br>- 테이블 간 관계를 설정하여 데이터 무결성 유지 |


## 명령어
```sql
-- 데이터베이스 확인하기
SHOW DATABASE;

-- 데이터베이스 생성하기
CREATE DATABASE [데이터베이스명];

-- 데이터베이스 선택하기
USE [데이터베이스명];

-- 테이블 만들기
CREATE TABEL [테이블명] ();

-- 테이블 확인하기(DESCRIBE)
DESC [테이블명];

-- 테이블 삭제하기
DROP TABLE [테이블명];

-- 컬럼 삭제하기
ALTER TABLE [테이블명] DROP [컬럼명];

-- 컬럼 수정하기
ALTER TABLE [테이블명] MODIFY COLUMN [컬럼명] [데이터타입] DEFAULT 0;
```

## DDL(Data Definition Language, 데이터 정의어)

| 명령어 | 설명 | 비고 |
| :----------: | --------- | ----------------- |
| **CREATE** | 데이터베이스, 테이블, 뷰(View), 인덱스 등 **새로운 객체를 생성** | `CREATE TABLE`, `CREATE DATABASE` 등 |
| **ALTER** | 기존 객체(테이블 등)의 **구조를 변경** | 컬럼 타입 변경, 칼럼 추가/삭제, 제약조건 추가 등 |
| **DROP** | 객체(테이블, 뷰 등)를 **완전히 삭제** | 데이터 + 구조 모두 삭제, 롤백 가능(DB 엔진 따라 다름) |
| **TRUNCATE** | 테이블의 모든 **데이터를 삭제**하지만, 구조는 유지 | **롤백 불가**, AUTO_INCREMENT 초기화 |

## DML(Data Manipulation Language, 데이터 조작어)

| 명령어 | 설명 | 비고 |
| :------: | ------------ | ------------------ |
| **SELECT** | 데이터를 조회(검색) | 조회만, 데이터 변경 없음 |
| **INSERT** | 데이터를 테이블에 삽입 | 새로운 행(row) 추가 |
| **UPDATE** | 데이터를 수정 | 조건을 지정하지 않으면 전체 수정 |
| **DELETE** | 데이터를 삭제 | 조건을 지정하지 않으면 전체 삭제 |

## MySQL 연산자

| 연산자 종류  | 연산자 | 설명 |
| :-------: | ----------- | ---------------- |
| 산술 연산자 | `+`, `-`, `*`, `/`, `MOD`, `DIV` | 숫자 계산에 사용 |
| 비교 연산자 | `=`, `<`, `>`, `<=`, `>=`, `<>` | 값의 비교 |
| 대입 연산자 | `=` | 값 할당 |
| 논리 연산자 | `AND`, `OR`, `NOT`, `XOR` | 조건 결합 |
| IS | `IS` | 양쪽 값이 같으면 `TRUE`, 아니면 `FALSE` |
| BETWEEN | `BETWEEN A AND B` | A 이상 B 이하이면 `TRUE`, 아니면 `FALSE` |
| IN | `IN` | 리스트 안에 값이 존재하면 `TRUE`, 아니면 `FALSE` |
| LIKE | `LIKE` | 패턴 매칭으로 문자열 검색, 존재하면 `TRUE`, 아니면 `FALSE` |

### 연산자 예제

```sql
-- 아이디가 apple인 유저의 아이디, 이름, 성별을 출력
SELECT userid, name, gender FROM member WHERE userid='apple';

-- 포인트가 200이상인 유저의 아이디, 이름, 포인트를 출력
SELECT userid, name, point FROM member WHERE point >= 200;

-- 포인트가 200이상 1000이하인 유저의 아이디, 이름, 포인트를 출력
SELECT userid, name, point FROM member WHERE point BETWEEN 200 AND 1000;
SELECT userid, name, point FROM member WHERE point >= 200 AND point <= 1000;

-- 아이디가 apple, orange, melon인 유저의 모든 정보를 출력
SELECT * FROM member WHERE userid='apple' OR userid='orange' OR userid='melon';
SELECT * FROM member WHERE userid IN ('apple', 'orange', 'melon');

-- 아이디가 'a'로 시작하는 유저의 모든 정보를 출력
SELECT * FROM member WHERE userid LIKE 'a%';
-- 아이디가 'a'로 끝나는 우저의 모든 정보를 출력
SELECT * FROM member WHERE userid LIKE '%a';
-- 아이디가 'a'를 포함하는 유저의 모든 정보를 출력
SELECT * FROM member WHERE userid like '%a%';

-- regdate 컬럼이 null인 데이터를 출력
SELECT * FROM voca WHERE regdate = 'NULL'; -- X
SELECT * FROM voca WHERE regdate = NULL; -- X
SELECT * FROM voca WHERE regdate IS NULL;

-- regdate 컬럼이 null이 아닌 데이터를 출력
SELECT * FROM voca WHERE regdate IS NOT NULL;
```
### 오른차순, 내림차순 예제

```sql
-- 멤버 가입순으로 오름차순 정렬하여 모든 정보를 출력
SELECT * FROM member ORDER BY idx; -- 오름차순
SELECT * FROM member ORDER BY idx ASC; -- 오름차순
SELECT * FROM member ORDER BY idx DESC; -- 내림차순

-- 멤버 테이블에서 포인트로 내림차순하고 포인트가 같다면 가입순으로 내림차순
SELECT * FROM member ORDER BY point DESC, idx DESC;
-- 멤버 테이블에서 포인트로 내림차순하고 포인트가 같다면 가입순으로 오름차순
SELECT * FROM member ORDER BY point DESC, idx ASC;
```
### LIMIT 예제

```sql
-- limit: 가져올 로우의 개수, limit 시작로우(인덱스) 가져올 로우의 개수
SELECT * FROM member LIMIT 2, 2;
-- 멤버 테이블에서서 포인트순으로 내림차순하고 포인트가 같다면 가입순으로 오름차순 한 뒤 top3를 출력
SELECT * FROM member ORDER BY point DESC, idx ASC LIMIT 3;
```
### GROUP BY, HAVING 예제

```sql
-- select 그룹을 맺은 컬럼 또는 집계함수 from 테이블 group by 그룹을 맺을 컬럼
-- select 그룹을 맺은 컬럼 또는 집계함수 from 테이블 group by 그룹을 맺을 컬럼 having 그룹에 대한 조건
-- 집계 함수: count(), sum(), avg(), min(), max()
SELECT gender FROM member GROUP Bㅕ gender;
-- 집계 함수에서 컬럼을 선택할 때 primary key 제약 조건이 있는 컬럼을 선택하는 것을 추천
SELECT gender, count(idx) as 인원 FROM member GROUP BY gender;
SELECT gender, count(idx) as 인원 FROM member GROUP BY gender HAVING 인원 >= 3;
```
### 총 활용 예제

```sql
-- 멤버 테이블에서 포인트가 200이상인 유저 중에서 성별로 그룹을 나눈 뒤 
-- 각 그룹의 포인트 평균을 구하고 평균의 포인트가 100이상인 성별을 출력
-- 단, 성별이 남자, 여자 모두 출력된다면 포인트가 높은 성별을 우선으로 출력
SELECT gender, avg(point) as avg FROM member 
WHERE point >= 200
GROUP BY gender Having avg >= 100 
ORDER BY avg DESC;
```

## 정규화(Normalization)와 비정규화(Denormalization)

### 정규화의 이점

> 같은 사실을 한 군데 저장하고, 나머지는 "키"로 연결      
> 즉, 데이터의 중복(Redundancy)이 줄고 관계(Relationship)을 설정하여 데이터의 일관성을 확보     
> 이상 현상(삽입/수정/삭제 시 발생하는 논리적 오류) 문제가 사라지며, 데이터의 품질과 확장성이 좋아짐      
> 원자값(쪼갤 수 없는 값, 제 1 정규화) + 함수 종속(제 2 정규화, 제 3 정규화) + 키로 연결(외래키)

### 정규화의 종류
#### 1️⃣ 제 1 정규화(1NF: First Normal Form)

> **정의**: 테이블의 모든 컬럼이 **원자값(Atomic Value)**을 가져야 한다.    
> 
> **의미**: 하나의 셀(Cell)에는 오직 하나의 값만 있어야 하며, 반복되는 그룹(Repeating Groups)을 허용하지 않는다.     
> 
> **예시**: 한 사람이 여러개의 취미를 가진 경우, 취미 칼럼에 "축구, 독서"처럼 여러 값을 콤마로 구분하지 않고, 각 취미를 별도의 레코드(행)로 분리해야 한다.    

#### 2️⃣ 제 2 정규화(2NF: Second Normal Form)

> **전제**: 1NF를 만족해야 한다.     
> 
> **정의**: 기본키(Primary Key)의 일부에만 종속되는 컬럼을 제거해야 한다. 즉, **완전 함수 종속(Full Functional Dependency)**을 만족해야 한다.      
> 
> **적용 대상**: 기본키가 **복합키(Composite Key)**일 때만 해당된다.    
> 
> **예시**: (주문번호, 상품 번호)가 복합키일 때, 상품 이름은 '주문번호'와는 관계없이 '상품번호'에만 종속된다. 이 경우, 상품 이름 컬럼을 별도의 '상품' 테이블로 분리한다.     

#### 3️⃣ 제 3 정규화(3NF: Third Normal Form)

> **전제**: 2NF을 만족해야 한다.      
> 
> **정의**: 기본키가 아닌 속성끼리도 서로 종속되면 안 된다. 즉, **이행적 함수 종속(Transitive Dependency)**을 제거해야 한다.       
> 
> **예시**: (학생 ID)가 기본키일 때, '학과 코드'는 (학생 ID)에 종속되고, '학과 이름'은 '학과 코드'에 종속된다. 이 경우, '학과 이름'은 기본키를 거쳐서 종속되는 이행적 종속이므로, '학과 코드'와 '학과 이름'을 별도의 '학과' 테이블로 분리한다.    

#### 비정규화(Denormalization)

> **정의**: 정규화된 데이터베이스를 성능 향상이라는 실무적인 목표를 위해 **의도적으로 정규화 원칙을 위배**하여 테이블을 다시 합치거나 데이터를 중복시키는 과정이다.      
> 
> **목적**: 데이터 조회 시 여러 테이블을 JOIN하는 횟수를 줄여 쿼리 속도를 향상시키기 위함이다.            
> 
> **적용 시점**: 정규화를 수행한 후, 실제 시스템의 성능 문제가 발생했을 때(특히 조회 성능), 또는 데이터베이스 설계 초기부터 조회 성능이 중요하다고 판단될 때 제한적으로 적용한다.         
>     
> **단점**: 데이터의 중복으로 인해 데이터 일관성(Integrity) 및 무결성(Consistency) 확보가 어렵고, 데이터 수정/삭제/삽입 시 발생하는 **이상 현상(Anomaly)**을 방지하기 위한 **추가적인 관리 및 처리(오버헤드)**가 필요하다.        

## MySQL JOIN의 종류와 목적
JOIN은 **두 개 이상의 테이블을 결합**하여 관련 데이터를 검색하는 데 사용된다.

### INNER JOIN

### LEFT JOIN

### RIGHT JOIN

### FULL OUTER JOIN

### CROSS JOIN

## 서브 쿼리(Subquery)
하나의 SQL 질의문 내부에 포함된 또 다른 SQL 질의문을 말하며, 부속질의라고도 불린다.

#### 목적

> 1. 메인 쿼리에 필요한 데이터를 조회하여 제공한다.       
> 2. 복잡한 쿼리를 논리적으로 분리하고 가독성을 높인다.       
> 3. `JOIN`으로는 해결하기 어려운 복잡한 조건을 처리한다.   

#### 종류(사용 위치에 따른 분류)
**SELECT절(스칼라 서브 쿼리)**: 하나의 값을 반환해야 하며, 주로 각 행에 대한 추가 정보 계산에 사용된다.
**FROM절(인라인 뷰)**: 서브 쿼리 결과를 하나의 임시 테이블(뷰)처럼 사용하며 메인 쿼리를 수행한다.
**WHERE절(일반 서브 쿼리)**: 메인 쿼리의 검색 조건으로 사용된다.
* **단일 행 서브쿼리**: 하나의 값만 반환하며 비교 연산자(`=, >, <`, 등)와 함께 사용된다.
* **다중 행 서브쿼리**: 여러 행을 반환하며 `IN, ANY, ALL`등 다중 행 연산자와 함께 사용된다.

