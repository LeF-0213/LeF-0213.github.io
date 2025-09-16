---
layout: post
related_posts:
    - /studylog/python
title:  "스택 / 큐"
date:   2025-09-14
categories:
  - studylog
  - python
description: >
  예시를 통해 
---
* toc
{:toc .large-only}

stack(FILO, LIFO)
1. push()
삽입
2. pop()
삭제

3~5단계 => stack + DFS => list
1~2단계 => stack(list)

stack
top = -1
↓
++top
stack push(stack, 1)
top = 0
↓
stack push(stack, 2)

stack에서 마지막으로 들어온 데이터를 가리키는 변수
=> int top = -1을 초기화한다

스택의 ADT
연산(함수), 속성(필드)

연산
1. boolean isFull() 파이썬은 동적이라 필요없음
2. boolean isEmpty()
3. void push(type, item)
4. type pop()
속성
1. int top
2. type data[max_size] 파이썬은 동적이라 필요없음

1. 빈 스택 생성
stack = []
2. 스택 사이즈 설정
max_size = 10
3. 함수 선언
```python
def isFull(stack):
  return len(stack) == max.size

def isEmpty(stack):
  return len(stack) == 0

void push(stack, item):
  if isFull(stack):
    print("stack이 가득 찼습니다.")
  else:
    stack.append(item)

void pop(stack):
  if isEmpty(stack):
    return None
  else:
    return append.pop()
```
↓
```python
stack = []

def push(stack, item):
  stack.append(item)

def pop(stack):
  if len(stack) == 0:
    return None
  else:
    return stack.pop()
```