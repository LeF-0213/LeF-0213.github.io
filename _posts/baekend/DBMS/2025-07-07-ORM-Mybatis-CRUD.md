---
layout: post
related_posts:
    - /baekend/dbms
title:  "MyBatis CRUD와 동적쿼리 작성"
date:   2025-07-02
categories:
  - baekend
  - spring
  - dbms
description: >
  Mybatis의 매핑구문
---
* toc
{:toc .large-only}


`https://mybatis.org/mybatis-3/ko/sqlmap-xml.html`
## Select 엘리먼트 속성
```java
    public List<TmpEntity> selectPersonByAgeFromTo(Integer a, Integer b);
```

```xml
    <select 
		id="selectPersonByAgeFromTo" 
		parameterType="Integer" 
		resultType="com.netflix.TmpEntity"
	>
    	SELECT * 
		FROM study3_person
		WHERE age BETWEEN #{a} AND #{b};
	</select>
```


```java
    @Mapper
    public interface EmployeeMapper {
        public void insertNewEmployee(Employee e);
    }
```

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
매개변수가 존재할 경우 parametertype으로 위치 지정
id에는 어떤 메소드와 연결할 것인지
namespace에는 어떤 클래스와 매핑해줄 것인지.

```xml
    <delete id="deletePersonById" parameterType="int">
		DELETE FROM study3_person
		WHERE id = #{id};
	</delete>
```

```xml
    <update id="updatePersonById" parameterType="com.netflix.TmpEntity">
		UPDATE study3_person
		SET age = #{age}, name = #{name}, join_date=#{joinDate}
		WHERE id = #{id};
	</update>
```

## Markup Not Recognized In Content
부등호를 썼을 때 일어날 수 있는 오류

XML 파일에서는 <를 `&lt;`로, >는 `&gt;` 로 써주는 게 안전
혹은 `<![CDATA[ ... ]]>` 안에서 ... 부분에 <, >, & 같은 특수문자를 넣어서 쓰는 것이 안전
ex) `<=`을 표기하고 싶은 경우
1. &lt;=
2. <![CDATA[ <= ]]>

# 동적쿼리
```xml
	<select 
		id="selectPersonByAgeFromTo2"
		parameterType="Integer" 
		resultType="com.netflix.entity.PersonEntity"
	>
		SELECT * FROM study3_person
		<if test="a != null and b != null">
			WHERE age BETWEEN #{a} AND #{b}		
		</if>
		<if test="a != null and b == null">
			WHERE age &gt;= #{a}
		</if>
		<if test="a == null and b != null">
			WHERE age &lt;= #{b}
		</if>
		;
	</select>
```