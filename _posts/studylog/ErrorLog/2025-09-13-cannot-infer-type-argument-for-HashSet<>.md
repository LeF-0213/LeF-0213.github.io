---
layout: post
related_posts:
    - /studylog/errorLog
title:  "error: cannot infer type arguments for HashSet<>"
date:   2025-09-13
categories:
  - studylog
  - errorLog
  - java
description: >
  Collections.reverseOrder() 사용시 타입 호환 에러
---
* toc
{:toc .large-only}

# 문제 발생
`error: cannot infer type arguments for HashSet<>`

# 문제 원인
HashSet<>에 타입 인자가 추론되지 않는 오류는 변수의 타입이 올바르지 않기 때문이다. 
이 오류를 해결하려면 HashSet 생성자에 List 형태의 컬렉션을 전달해야 한다 
이를 위해서는 원시 타입 배열 arr을 `Arrays.asList(arr)` 또는 `List.of(arr)`와 같이 List로 변환해야 합니다. 

