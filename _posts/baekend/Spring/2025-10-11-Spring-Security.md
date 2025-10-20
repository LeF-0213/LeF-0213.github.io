---
layout: post
related_posts:
    - /baekend/spring
title:  "spring boot에서 사용하는 annotaion"
date:   2025-10-11
categories:
  - baekend
  - spring
description: >
  spring boot에서 사용하는 어노테이션
---
* toc
{:toc .large-only}

## 프로젝트 구조
```
src/main/java/com/example/Project/
├── config/
│   └── SecurityConfig.java
├── controller/
...
```
## 의존성 추가
### build.gradle(Gradle)
```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
}
```
### pom.xml(Maven)
```xml
xml<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
## 파일 생성
### config/SecurityConfig.java
