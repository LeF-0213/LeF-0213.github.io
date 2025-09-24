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

## Queue(FIFO)
enqueue() => => dequeue()
데이터를 가리키는 변수
front = 가장 먼저 들어온 데이터를 가리킨다
rear =  가장 나중에 들어온 데이터를 가리킨다.

front = -1
rear = -1
enqueue() => 1 => dequeue()
front = 1
rear = 1
enqueue() => 2, 1 => dequeue()
front = 1
rear = 2
enqueue() => 5, 4, 3, 2 => dequeue()
front = 2
rear = 5
fornt = 4
rear = 5
enqueue() => 5, 4       => dequeue()
queue는 데이터가 삭제된 영역은 사용할 수 없다.

덱을 큐처럼 활용하기 위해서는
```python
from collections import deque

queue = deque()

# 큐에 데이터를 삽입
queue.append(1)
queue.append(2)
queue.append(3)

# 큐에서 데이터를 삭제(popleft())
```

큐를 구현하는데 두 가지 방법이 존재한다.
1. 리스트로 활용 => append(), pop()(스택과의 차이점은 pop() 메서드에 인수로 0을 넣는다.)
2. 덱을 이용하여 활용 => 
일반적으로 큐는 리스트보다는 덱을 사용하여 활용한다.

Queue
deque
문제

for _ in range(): 저장되지 않은 값을 사용할 때 이용하는 것
for i in rage(): for 변수 in range()
naming => _, __

문제] _main_ 사용 의미

Queue

연산 
1. boolean isFull()  큐의 maxsize인지 확인
2. boolean isEmpty() 큐의 데이터가 하나도 없는지 확인
3. void push(itemType item)
4. itemType pop()

상태(변수) 
1. int front 처음 들어온 데이터를 가리킨다
2. int rear 마지막으로 들어온 데이터를 가리킨다.
3. itemType data[maxSize] 데이터를 관리하는 배열

초기값 front, rear = -1

1. isFull() 함수를 이용하여 큐에 가득찼는지 확인 만약 False를 반환하면 데이터를 집어 넣는다. 그러나 먼저 해야 할 것은 rear + 1
(파이썬은 동적 배열을 사용하기 때문에 이러한 방식은 사용하지 않는다.)

큐를 구현하는 방법으로는 두가지
1. 리스트를 큐처럼 사용 => push() 연산은 append()
2. deque를 큐처럼 사용

1. 리스트를 큐처럼 사용
```python
queue = []
# 큐에 데이터 추가
queue.append(1)
queue.append(2)
queue.append(3)

# 큐를 제거
item = queue.pop(0)
```
2. 덱을 활용하는 방법
```python
from collections import deque

# 큐에 데이터 추가
queue.append(1)
queue.append(2)
queue.append(3)

# 큐를 제거
item = queue.popleft()
```

https://www.acmicpc.net/problem/1158 https://school.programmers.co.kr/learn/courses/30/lessons/42586 https://school.programmers.co.kr/learn/courses/30/lessons/159994 https://school.programmers.co.kr/learn/courses/30/lessons/1835