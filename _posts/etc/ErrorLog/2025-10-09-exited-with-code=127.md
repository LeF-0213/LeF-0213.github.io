---
layout: post
related_posts:
    - /etc/errorlog
title:  "exited with code=127 오류"
date:   2025-10-09
categories:
  - etc
  - errorlog
  - python
description: >
  
---
* toc
{:toc .large-only}

## 문제 발생
**Exit code 127**은 `"command not found"`를 의미한다. 즉 VSCode의 Code Runner 확장에서 파이썬 인터프리터를 찾지 못했을 때 발생하는 오류이다.

## 해결방법
### 파이썬이 설치되어 있는지 확인
터미널에서 확인:
```bash
bashpython --version
# 또는
python3 --version
```
### Code Runner 설정 수정
VSCode 설정에서 파이썬 경로를 직접 지정:
1. command + shift + p (설정 열기)
2. "code-runner.executorMap" 검색
3. settings.json에서 직접 편집:
```json
json{
    "code-runner.executorMap": {
        "python": "python3 -u"
    }
}
```
또는 전체 경로 지정:
```json
{
    "code-runner.executorMap": {
        "python": "C:\\Python312\\python.exe -u"  // Windows
        // 또는
        "python": "/usr/bin/python3 -u"  // Mac/Linux
    }
}
```