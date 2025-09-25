---
layout: post
related_posts:
    - /studylog/errorlog
title:  "[Vue.js] Parsing error: No Babel config file detected 에러"
date:   2025-07-10
categories:
  - studylog
  - errorlog
  - spring
  - db
description: >
  [Vue.js] Parsing error: No Babel config file detected 에러 해결방법
---
* toc
{:toc .large-only}

## 문제 원인
ESLint가 Babel 설정 파일을 찾지 못해서 발생하는 문제로, VSCode에서 프로젝트의 ESLint 설정 파일을 인식하는 부분에 발생된 에러이다.

## 해결
1. VS Code에서 `Ctrl + Shift + P` 누르기
2. `Preferences: Open Workspace Settings (JSON)`을 입력하고 파일 열기
3. 
  ```js
    "eslint.workingDirectories": [
      {
        "mode": "auto"
      }
    ]
  ```
  를 {}안에 붙여넣기

![parsingErrorImg1](/blog/assets/img/studyImg/parsingErrorImg1.png)
이 경로에서
![parsingErrorImg2](/blog/assets/img/studyImg/parsingErrorImg2.png)
위 그림과 같이 붙여넣고 저장하면 오류가 해결된다.