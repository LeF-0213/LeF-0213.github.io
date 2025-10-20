---
layout: post
related_posts:
    - /baekend/spring
title:  "spring boot에 DB 연결하기(MySQL)"
date:   2025-06-25
categories:
  - baekend
  - spring
  - db
description: >
  MySQL을 spring boot와 연결하는 방법
---
* toc
{:toc .large-only}



# Spring Boot와 연결
1. spring boot 프로젝트에 MySQL Connecter를 설치해야 한다.
`build.gradle` 파일의 `dependencies` 부분에 설치하고자 하는 대상을 입력
```java
    dependencies {
        implementation 'mysql:mysql-connector-java:8.0.32'
        implementation 'org.springframework.boot:spring-boot-stater-jdbc'
    }
```
**주의할 점**
- 버전 정보는 다운로드한 버전에 맞게 변경해주기
- 혹은 자신의 상황에 맞게 `Maven Repository(MVNREPOSITORY)`에서 검색
2. build.gradle을 `refresh`해서 해당 파일을 설치해주기
   eclipse의 경우 build.gradle 파일 마우스 우클릭 > gradle > refresh gradle project
   intellij의 경우 코끼리 모양 버튼 클릭
3. 연결 접속 정보 설정(user, pw)
   `application.properties` 파일에 연결한 Database이름, 계정 정보, 비밀번호를 설정해야 한다.

   ```java
        spring.datasource.url=jdbc:mysql://localhost(DB주소):3306(PORT)/DB이름?useSSL=false&serverTimezone=UTC&useLegacyDatetime
		spring.datasource.username=계정이름
		spring.datasource.password=비밀번호
   ```

useSSL=false:
SSL(Secure Sockets Layer)은 데이터 전송 시 보안을 강화하는 프로토콜입니다. 하지만 개발 및 테스트 환경에서는 SSL을 사용하지 않는 것이 일반적입니다. 이 설정은 MySQL 서버와 클라이언트 간의 SSL 암호화 연결을 비활성화합니다. 만약 MySQL 서버가 SSL을 요구하도록 설정되어 있다면 이 설정을 사용해야 합니다.
serverTimezone=UTC:
MySQL 서버의 타임존을 UTC(협정 세계시)로 설정합니다. 데이터베이스에 저장되는 날짜 및 시간 데이터는 서버의 타임존 설정에 영향을 받습니다. UTC는 표준 시간대이므로, 여러 지역에서 접속하는 애플리케이션의 경우 일관성을 유지하기 위해 UTC를 사용하는 것이 좋습니다. 한국에서 접속하는 경우 serverTimezone=Asia/Seoul과 같이 설정할 수도 있습니다.
useLegacyDatetime:
MySQL 5.6 이전 버전과의 호환성을 위해 사용되는 설정입니다. MySQL 5.7 이상 버전에서는 날짜 및 시간 데이터 타입이 개선되었지만, 이전 버전과의 호환성을 유지하기 위해 이 설정을 사용할 수 있습니다. 이 설정을 사용하면 날짜 및 시간 데이터가 이전 버전과 동일하게 처리됩니다. 

4. 연결한 방식 설정(JPA, MyBatis)
**JPA**

**MyBatis**
```build.gradle
implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.0'
```

5. MyBatis 관련 설정을 application.properties 파일에 해준다
```application.properties
mybatis.mapper-locations=classpath:/mappers/**/*.xml
```
  보통은 `src/main/resources` 안에 `mappers`라는 폴더를 새로 만들어 그 속에 저장한다. (`src/main/resources/mappers`)

## xml 파일
태그로 정보를 전달하는 파일
전달하고자 하는 값들이 태그 형식으로 이루어진 파일
xml 파일의 맨 윗부분에는 해당 파일이 어떤 파일인지 알려주는 태그를 사용해야 한다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
```
두번째 줄에는 문서의 종류를 작성해줘야 한다.
```xml    
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD MyBatis 3 Mapper//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

### 🖍️ 발생 가능한 오류
* `Downloading external resources is disabled. [DownloadResourceDisabled]` 
위 오류 발생했다면, IDE에서 외부 리소스 다운로드 허용해주어야 합니다.
- IntelliJ 기준
`Preferences (Settings) → Languages & Frameworks → Schemas and DTDs → XML Catalog` \n
혹은: `Editor > Inspections → Download external resources 비활성화`
- Eclipse 기준
빨간 밑줄 위에 마우스를 올려 `Force download of the DTD resource` 클릭 \n
혹은 `설정 > Preferences > XML > XML Catalog` 에서 수동 등록하거나, 다운로드 관련 기능 제한을 해제

xml 파일에서의 주석
``` xml
  <!-- 주석 -->
```

Attribute "id" is required and must be specified for element type "select". [MSG_REQUIRED_ATTRIBUTE_NOT_SPECIFIED]	

인테페이스를 만든다.

###  `Invalid value type for attribute 'factoryBeanObjectType': java.lang.String`
**문제 발생**
build.gradle
```java 
id 'org.springframework.boot' version '3.4.7'
dependencies {
  implementation 'mysql:mysql-connector-java:8.0.32'
  implementation 'org.springframework.boot:spring-boot-starter-jdbc'
  implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.0'
}
여기서 `Invalid value type for attribute 'factoryBeanObjectType': java.lang.String` 오류 발생

**문제 원인**
spring boot 3버전부터는 mysql 연결하는 방식 변경

**해결 방법**
해결 방법은 두 가지가 존재한다.
spring boot 3버전부터는 mybatis로 mysql 연결하는 방식 변경되어 일어나는 문제이니
3버전 이후부터는 `implementation 'mysql:mysql-connector-java:8.0.32'`에서 `runtimeOnly 'com.mysql-connector-j'`로 변경해주면 된다.
```java
implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.3'
runtimeOnly 'com.mysql:mysql-connector-j'
```

다른 하나는 pring boot와 mybatis의 버전이 달라서 일어나는 문제이므로
https://github.com/mybatis/spring-boot-starter/releases를 통해 확인해서 알맞게 바꿔주면 해결된다.
```java
  id 'org.springframework.boot' version '3.4.7'
  dependencies {
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.4'
  }
``` 

### `Error creating bean with name 'sqlSessionFactory' defined in class path resource`
xml 파일에서 <mapper> 태그에 namespace로 경로를 지정해주면 해결된다.
`<mapper namespace="com.example.mapper.EmployeeMapper"></mapper>`

## `mybatis.configuration.map-underscore-to-camel-case=true`
application.properties에 추가해주지 않으면
데이터베이스에서 join_date와 같이 자바의 카멜표기법과 달라서 매칭이 되지 않고 null이 출력될 수 있다.