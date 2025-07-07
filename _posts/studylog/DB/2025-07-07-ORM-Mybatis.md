---
layout: post
related_posts:
    - /studylog/db
title:  "MyBatis 매핑구문"
date:   2025-07-02
categories:
  - studylog
  - spring
  - db
description: >
  Mybatis의 매핑구문
---
* toc
{:toc .large-only}


`https://mybatis.org/mybatis-3/ko/sqlmap-xml.html`
## Select 엘리먼트 속성



```xml
    <insert id="insertNewEmployee" parameterType="com.netflix.Employee">
		INSERT INTO employee
		(first_name, last_name, email, phone_number, hire_date, salary, department, manager_id, date_of_birth)
		VALUES
		(
			#{firstName}, 
			#{lastName}, 
			#{email}, 
			#{phoneNumber}, 
			#{hireDate}, 
			#{salary}, 
			#{departement},
			#{managerId},
			#{dateOfBirth} 
		);
	</insert>
```