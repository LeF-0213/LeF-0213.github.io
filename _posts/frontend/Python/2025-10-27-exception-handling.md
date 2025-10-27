---
layout: post
related_posts:
    - /frontend/python
title:  "[Python] 예외 처리(Exception Handling)"
date:   2025-10-27
categories:
  - frontend
  - python
description: >
  
---
* toc
{:toc .large-only}

# 예외란?
예외(Exception)는 프로그램 실행 중 발생할 수 있는 예상치 못한 문제 또는 오류 상황을 의미합니다. 예외가 발생하면 프로그램은 중단되기 때문에 이를 적절하게 처리하여 중단을 방지하거나 오류에 대한 정보를 사용자에게 제공해야 합니다.

## 예외가 발생하는 경우
### Value Error

### TypeError

### ZeroDivisionError

### IndexError

### KeyError

### AttributeError

### FileNotFoundError

### ImportError

### NameError

### OverflowError

### MemoryError

## 예외 처리 기본 구조

```python
try:
    # 예외가 발생할 가능성이 있는 코드
except ExceptionType1:  # 'ExceptionType1'에는 실제 예외 유형이 들어갑니다.
    # ExceptionType1 예외가 발생했을 때 실행될 코드
except ExceptionType2:  # 'ExceptionType2'에는 다른 예외 유형이 들어갑니다.
    # ExceptionType2 예외가 발생했을 때 실행될 코드
# 추가적인 except 블록을 계속 추가할 수 있습니다.
else:
    # try 블록에서 예외가 발생하지 않았을 때 실행될 코드
finally:
    # 예외 발생 여부와 관계없이 항상 실행될 코드
```

## Exception 클래스
Exception 클래스는 파이썬의 내장 예외 계층 구조에서 거의 모든 내장 예외의 기본 클래스입니다. 이 클래스는 사용자 정의 예외를 만들거나 특정 예외 유형을 잡기 위한 기본적인 인터페이스를 제공합니다. 예외 유형은 Exception을 상속받아서 정의됩니다. 예를 들면 ValueError, TypeError, FileNotFoundError 등이 있습니다. 이 상속 구조 덕분에 except Exception 블록은 Exception을 상속받은 모든 예외를 처리할 수 있습니다.

```python
# raise: 예외 상황을 발생시킴
try:
    raise Exception('예외가 발생했어요!!')
except Exception as e:
    print(e)
```

## 사용자 정의 예외 클래스를 직접 만들고 활용
