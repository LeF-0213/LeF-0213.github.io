---
layout: post
related_posts:
    - /frontend/python
title:  "자료구조(선형 자료구조)"
date:   2025-08-20
categories:
  - frontend
  - python
description: >
  Python 3 자료구조(선형 자료구조) 구현 방법 설명
---
* toc
{:toc .large-only}

# 자료구조(Data Structure)란?
* 데이터를 **저장**하고 **탐색**하기 위한 구조
* 크게 두 가지로 나뉨:
  * **선형 자료구조**: 배열, 연결리스트, 스택, 큐
  * **비선형 자료구조**: 트리, 그래프
* 파이썬에서는 배열 대신 **리스트(list)** 사용

## ADT(Abstract Data Type, 추상 자료형)란?
* 자료구조에서 **데이터와 연산을 논리적으로 정의한 것**
* **구현 방법과 무관하게**, **어떤 연산이 가능한지만 정의**
### 구성 요소
* **속성(Attribute)**: 데이터의 상태(예: 리스트의 크기, 원소 값)
* **연산(Operation/Method)**: 데이터에 적용 가능한 기능(예: 삽입 `insert()`, 삭제 `remove()`, 탐색 `find()`)

# 선형 자료구조
**데이터의 저장방식**: 데이터를 정해진 순서대로 저장 및 탐색
## 배열 리스트(Array/List)
* 데이터를 순차적으로 나열한 구조
* Python에서는 **리스트(list)**가 배열 역할을 한다.
```python
# 빈 리스트
a = []

# 초기화
arr = [10, 20, 30, 40]
arr = [0] * 6
arr = list(range(6)) # [0, 1, 2, 3, 4 ,5]
arr = [0 for _ in range(6)]
```
### 주요 연산
* 삽입: append(), insert()
* 삭제: pop(), remove(), clear()
* 기타: sort(), reverse(), count(), index()

## 연결 리스트(Linked List)
* 데이터를 **노드(Node)** 단위로 저장, 포인터(link)로 연결하여 관리
* **장점**: 동적 할당(크기 제한이 없음)
* **단점**: 탐색이 느림 `O(n)`
```python
class ListNode:
	def _init_(self, item, next = None):
		self.item = item
		self.next = next
```
* **삽입 위치**: 맨 앞, 맨 뒤, 중간
* 파이썬 리스트보다 구현이 복잡하지만 공간 효율적
### 삽입 동작 원리
연결 리스트에서 삽입은 **포인터(주소) 재배치**로 이루어진다.
* 노드 구조: `데이터(item)` + `주소(next)`
* 헤더(dummy): 데이터는 없고 첫 노드의 주소만 가진다.(`item` 값이 없고, `link`만 존재)
```python
if i == 0:
	# 맨 앞에 삽입할 때
	newNode.item = x
	newNode.next = head
	head = newNode
else:
	# 중간에 삽입할 때
	newNode.item = x
	newNode.next = prev.next
	prev.next = newNode
```
* **새 노드가 기존 연결고리를 끊지 않고 끼어드는 방식이다.**
* 만약 `i == 0`이라면 `head`가 새 노드를 가리키도록 갱신한다.

## 스택(Stack, LIFO/FILO)
* 특징: **마지막에 들어온 원서가 먼저 나감(Last In First Out/ Frist In Last Out)**
### 스택의 ADT
* 속성 : 
  * int `top`: 마지막으로 들어온 데이터 인덱스
  * type `data`[max_size]: 스택의 데이터 저장 공간(파이썬은 동적이라 필요없음)
* 주요 연산 : 
  * void `push()`: 삽입
  * type `pop()`: 삭제
  * boolean `isEmpty()`: 비어있는지 확인
  * boolean `isFull()`: 가득 찼는지 확인 (파이썬은 동적이라 필요없음)
### 코드 예시
```python
stack = []
max_size = 10

def isFull(stack):
    return len(stack) == max_size

def isEmpty(stack):
    return len(stack) == 0

def push(stack, item):
    if isFull(stack):
        print("stack이 가득 찼습니다.")
    else:
        stack.append(item)

def pop(stack):
    if isEmpty(stack):
        print("stack이 비어있습니다.")
        return None
    else:
        return stack.pop()
```
### 구현 예시
```python
# 빈 스택 생성
stack = []

# 원소 삽입
def push(stack, item):
  stack.append(item)

# 원소 삭제
def pop(stack):
  if len(stack) == 0:
    return None
  else:
    return stack.pop()
```
### 사용 예시 - 올바른 괄호 판멸
```python
def isParentheses(s):
    stack = []
    for ch in s:
        if ch == '(':
            stack.append(ch)
        else:
            if not stack:
                return False
            stack.pop()
    return len(stack) == 0
```
### 사용 예시 - 문자열 짝 제거
```python
def alpabet(s):
    stack = []
    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)
    return int(not stack)
```

## 큐(Queue, FIFO)
* 특징: **먼저 들어온 원소가 먼저 나감(First In First Out)**
### 큐의 ADT
* 속성:
  * `queue`: 원소 저장 리스트
  * `front`: 제일 처음 들오온 데이터를 가리킴
  * `rear`: 마지막으로 들어온 데이터를 가리킴
* 주요 연산:
  * `enQueue()`: 삽입
  * `deQueue()`: 삭제
### 구현 예시
```python
from collections import deque

queue = deque()

queue.append(1) # enQueue
queue.append(2)
item = queue.popleft() # deQueue
```
### 삽입/삭제 동작 원리
큐는 스택과 비슷하지만 **삭제 방향이 앞(front)**에서 이루어진다.
```python
from collections import deque

# 빈 큐 생성: queue = [] 또는 queue = deque()
queue = deque()
# front / rear 초기화
front = rear = -1
# 원소 삽입(enQueue)
rear += 1
queue.append(item) # 마지막에 원소 추가
# 원소 삭제(deQueue)
front += 1
item = queue.pop() # 맨 앞 원소 삭제
```
* 리스트를 이용하면 삭제 후 공간을 재사용할 수 없고 속도가 느리다.
* 이를 해결하기 위해 `deque` 사용.
### 리스트로 구현
```python
queue = []

def enQueue(queue, item):
	queue.append(item) # 마지막에 추가

def deQueue(queue):
	if len(queue) == 0:
		print("큐가 비어있습니다.")
		return None
	else:
		return queue.pop(0) # 맨 앞 원소 삭제
```
### dequeue로 구현
```python
from collections import deque

queue = deque()

def enQueue(queue, item):
	queue.append(item) # 마지막에 추가

def deQueue(queue):
	if len(queue) == 0:
		print("큐가 비어있습니다.")
		return None
	else:
		return queue.popleft() # 맨 앞 원소 삭제
```
### 사용 예시 - 카드 뽑기 문제
```python
from collections import deque

def solution(card1, card2, goal):
  card1 = deque(card1)
  card2 = deque(card2)
  goal = deque(goal)

  while goal: # goal의 문자열을 순차적으로 작업
    if card1 and card1[0] == goal[0]:
      card1.popleft()
      goal.popleft()
    elif card2 and card2[0] == goal[0]:
      card2.popleft()
      goal.popleft()
    else:
      break

  if not goal:
    return 'Yes'
  else:
    return 'No' 
```

