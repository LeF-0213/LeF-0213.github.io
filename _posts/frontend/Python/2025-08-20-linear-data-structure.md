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
* 예: 스택은 "push", "pop"이 가능하다는 것만 정의 (내부가 배열인지 연결리스트인지는 무관)

### 구성 요소
* **속성(Attribute)**: 데이터의 상태(예: 리스트의 크기, 원소 값)
* **연산(Operation/Method)**: 데이터에 적용 가능한 기능(예: 삽입 `insert()`, 삭제 `remove()`, 탐색 `find()`)

### ADT vs 자료구조

| 구분 | ADT (추상 자료형) | 자료구조 (Data Structure) |
|------|------------------|-------------------------|
| 정의 | 논리적 개념 | 물리적 구현 |
| 예시 | Queue(큐) | deque, list로 구현된 큐 |
| 관심사 | "무엇을" 할 수 있는가 | "어떻게" 구현하는가 |

# 선형 자료구조
**데이터의 저장방식**: 데이터를 정해진 순서대로 저장 및 탐색

## 배열 리스트(Array/List)
![array](https://wikidocs.net/images/page/189478/fig-010.png)
* 데이터를 **연속된 메모리 공간**에 순차적으로 나열한 구조
* Python에서는 **리스트(list)**가 동적 배열 역할을 한다.

### 특징
* **장점**:
  * 인덱스를 통한 빠른 접근 `O(1)`
  * 간단하고 직관적인 구조
  * 메모리 상에서 연속적으로 저장
* **단점**:
  * 중간 삽입/삭제 시 데이터 이동 필요 `O(n)`
  * 크기 변경 시 재할당 필요 (파이썬은 동적으로 처리)

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

### 시간 복잡도

| 연산 | 시간 복잡도 |
|------|-----------|
| 접근 (arr[i]) | `O(1)` |
| 끝에 추가 (append) | `O(1)` |
| 중간 삽입 (insert) | `O(n)` |
| 끝에서 삭제 (pop) | `O(1)` |
| 중간 삭제 (pop(i)) | `O(n)` |
| 탐색 (in, index) | `O(n)` |

## 연결 리스트(Linked List)
![linkedlist](https://wikidocs.net/images/page/224937/fig-016_.png)
* 데이터를 **노드(Node)** 단위로 저장
* 각 노드는 **데이터(item)**와 **다음 노드의 주소(next)**를 가짐
* 노드들이 **포인터(link)**로 연결되어 관리
* 단일 연결 리스트와 양방향(더블) 연결 리스트로 나뉜다.[단일 연결리스트 vs 양방향 연결리스트]()

### 특징
* **장점**:
  * 동적 크기 할당 (메모리 효율적)
  * 중간 삽입/삭제가 빠름 `O(1)` (위치를 알고 있을 때)
  * 크기 제한 없음
* **단점**:
  * 인덱스 접근 불가 (순차 탐색 필요) `O(n)`
  * 추가 메모리 필요 (포인터 저장)
  * 구현이 복잡

### 노드 구조

```python
class ListNode:
	def _init_(self, item, next = None):
		self.item = item
		self.next = next

# 노드 생성
node1 = ListNode(10)
node2 = ListNode(20)
node3 = ListNode(30)

# 연결
node1.next = node2
node2.next = node3
```
### 연결 리스트 클래스 구현

```python
class LinkedList:
  def __init__(self):
    self.head = None # 첫 노드를 가리킴
    self.size = 0

  def is_empty(self):
    return self.size == 0

  def get_size(self):
    return self.size
```

### 삽입 연산
연결 리스트에서 삽입은 **포인터(주소) 재배치**로 이루어진다.
* **삽입 위치**: 맨 앞, 맨 뒤, 중간
* 파이썬 리스트보다 구현이 복잡하지만 공간 효율적
* 노드 구조: `데이터(item)` + `주소(next)`
* 헤더(dummy): 데이터는 없고 첫 노드의 주소만 가진다.(`item` 값이 없고, `link`만 존재)

#### 맨 앞에 삽입할 때

```python
def inser_front(self, item):
	new_node = ListNode(item)
	new_node.next = self.head # 새 노드가 기존 head를 가리킴
	self.head = new_node      # head를 새 노드로 갱신
  self.size += 1

# 동작 원리:
# 기존: head -> [10] -> [20] -> None
# 삽입: [5] -> [10] -> [20] -> None
# 새로운 head: [5]
```
#### 맨 뒤에 삽입

```python
def insert_back(self, item):
  new_node = ListNode(item)

  if self.is_empty():
    self.head = new_node
  else:
    current = self.head
    while current.next: # 마지막 노드까지 이동
      current = current.next
    current.next = new_node # 마지막 노드가 새 노드를 가리킴
  self.size += 1

# 동작 원리:
# 기존: head -> [10] -> [20] -> None
# 삽입: head -> [10] -> [20] -> [30] -> None
```
#### 특정 위치에 삽입

```python
def insert_at(self, index, item):
  if index < 0 or index > self.size:
    raise IndexError("Index out of range")

  if index == 0:
    self.insert_front(item)
    return

  new_node = ListNode(item)
  current = self.head

  # index-1 위치까지 이동
  for _ in range(index - 1):
    current = current.next

  # 포인터 재배치
  new_node.next = current.next  # 새 노드가 다음 노드를 가리킴
  current.next = new_node       # 이전 노드가 새 노드를 가리킴
  self.size += 1
```
#### 삭제 연산

```python
# 맨 앞 노드 삭제
def delete_front(self):
  if self.is_empty():
    raise Exception("List is empty")

  deleted_item = self.head.item
  self.head = self.head.next  # head를 다음 노드로 이동
  self.size -= 1
  return deleted_item

# 특정 위치 노드 삭제
def delete_at(self, index):
  if index < 0 or index >= self.size:
    raise IndexError("Index out of range")
    
    if index == 0:
        return self.delete_front()
    
    current = self.head
    # index -1 위치까지 이동
    for _ in range(index - 1):
        current = current.next

    deleted_item = current.next.item
    current.next = current.next.next  # 삭제할 노드를 건너뜀
    self.size -= 1
    return deleted_item
```
#### 탐색 연산

```python
# 값 탐색
def search(self, item):
  current = self.head
  index = 0

  while current:
    if current.item == item:
      return index
    current = current.next  # 발견할 때까지 동작
    index += 1

  return -1 # 찾지 못함

# 인덱스로 값 가져오기
def get_at(self, index):
  if index < 0 or index >= self.size:
    raise IndexError("Index out of range")

  current = self.head
  for _ in range(index):
    current = current.next

  return current.item
```
### 시간 복잡도

| 연산 | 배열 | 연결 리스트 |
|------|------|-----------|
| 접근 (index) | O(1) | O(n) |
| 맨 앞 삽입 | O(n) | O(1) |
| 맨 뒤 삽입 | O(1) | O(n) |
| 중간 삽입 | O(n) | O(n) |
| 맨 앞 삭제 | O(n) | O(1) |
| 맨 뒤 삭제 | O(1) | O(n) |
| 탐색 | O(n) | O(n) |

### 더미 헤드 (Dummy Head)
* **더미 헤드**: 데이터는 없고 첫 노드의 주소만 가진 가상의 노드
* 삽입/삭제 시 경계 조건 처리를 단순화

```python
class LinkedListWithDummy:
  def __init__(self):
    self.dummy = ListNode(None)  # 더미 헤드
    self.size = 0

  # 더미 헤드를 사용한 삽입
  def insert_at(self, index, item):
    if index < 0 or index > self.size:
      raise IndexError("Index out of range")

    new_node = ListNode(item)
    current = self.dummy

    # index 위치까지 이동
    for _ in range(index):
      current = current.next

    # 포인터 재배치
    new_node.next = current.next
    current.next = new_node
    self.size += 1
```
### 배열 vs 연결 리스트 선택 기준
* **배열 사용**: 인덱스 접근이 많고, 크기가 고정적인 경우
* **연결 리스트 사용**: 삽입/삭제가 빈번하고, 크기가 동적인 경우
* **참고**: 파이썬의 list는 동적 배열로 구현되어 있어, 대부분의 경우 연결 리스트보다 효율적이다

## 스택(Stack, LIFO/FILO)
* 특징: **마지막에 들어온 원서가 먼저 나감(Last In First Out/ Frist In Last Out)**
* 한쪽 끝(top)에서만 삽입과 삭제가 일어나는 선형 자료구조

![stack](https://wikidocs.net/images/page/198491/stack.png)

### 스택의 ADT
* 속성 : 
  * int `top`: 마지막으로 들어온 데이터 인덱스
  * type `data`[max_size]: 스택의 데이터 저장 공간(파이썬은 동적이라 필요없음)
* 주요 연산 : 
  * void `push()`: 스택의 맨 위에 원소 삽입
  * type `pop()`: 스택의 맨 위 원소 삭제 및 반환
  * type `peeek()`: 스택의 맨 위 원소 확인 (삭제하지 않음)
  * boolean `isEmpty()`: 비어있는지 확인
  * boolean `isFull()`: 가득 찼는지 확인 (파이썬은 동적이라 필요없음)
  * int `size()`: 스택의 크기 반환

### 시간 복잡도
* push: `O(1)`
* pop: `O(1)`
* peek: `O(1)`
* isEmpty: `O(1)`

### 클래스로 구현 예시

```python
class Stack:
  def __init__(self, max_size=None):
    self.items = []
    self.max_size = max_size

  def push(self, item):
    if self.max_size and len(self.items) >= self.max_xize:
      raise Exception("Stack overflow")
    self.items.append(item)

  def pop(self):
    if self.is_empty():
      raise Exception("Stack underflow")
    return self.items.pop()

  def peek(self):
    if self.is_empty():
      raise Exception("Stack is empty")
    return self.items[-1]

  def is_empty(self):
    return len(self.items) == 0

  def size(self):
    return len(self.items)
```
### 활용 예시 - 올바른 괄호 판멸
* 여러 종류의 괄호가 올바르게 닫혔는지 확인
* 예: "({[]})" → True, "({[}])" → False

```python
def isParentheses(s):
    stack = []
    pairs = {'(': ')', '{': '}', '[': ']'}

    for ch in s:
        if ch in pairs: # 여는 괄호
            stack.append(ch)
        else: # 닫는 괄호
            if not stack or pairs[stack.pop()] != ch:
                return False

    return len(stack) == 0

# 테스트
print(isValidParentheses("()[]{}"))  # True
print(isValidParentheses("([)]"))    # False
print(isValidParentheses("{[]}"))    # True
```
### 활용 예시 - 문자열 짝 제거
* 연속된 같은 문자를 제거
* 예: "abbaca" → "ca"

```python
def removeAdjacentDuplicates(s):
    stack = []

    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)
    
    return ''.join(stack)

print(removeAdjacentDuplicates("abbaca"))  # "ca"
print(removeAdjacentDuplicates("azxxzy"))  # "ay"
```
### 활용 예시 - 브라우저 뒤로가기/앞으로가기
```python
class Browser:
  def __init__(self):
    self.back_stack = []
    self.forward_stack = []
    self.current = None

  def visit(self, url):
    if self.current:            # current가 None이 아니었을 때 뒤로가기 기록에 추가 그리고 current url을 visit한 새 페이지 url로 바꾸기
      self.back_stack.append(self.current)
    self.current = url
    self.forward_stack.clear()  # 새 페이지 방문 시 앞으로 가기 기록 삭제
    print(f'방문: {url}')

  def back(self):
    if not self.back_stack:
      print("뒤로 갈 페이지가 없습니다.")
      return
    self.forward_stack.append(self.current) # 뒤로가기가 실행되면 앞으로가기 버튼에 현재 페이지 추가
    self.current = self.back_stack.pop() # 현재 페이지를 뒤로가기의 가장 최신으로 바꾸기
    print(f'뒤로가기: {self.current}')

  def forward(self):
    if not self.forward_stack:
      print("앞으로 갈 페이지가 없습니다.")
      return
    self.back_stack.append(self.current)
    self.current = self.forward_stack.pop()
    print(f'앞으로가기: {self.current}')

# 테스트
browser = Browser()
browser.visit("google.com")
browser.visit("youtube.com")
browser.visit("github.com")
browser.back()      # youtube.com
browser.back()      # google.com
browser.forward()   # youtube.com
```

## 큐(Queue, FIFO)
* 특징: **먼저 들어온 원소가 먼저 나감(First In First Out)**
* 한쪽 끝(rear)에서 삽입, 다른 쪽 끝(front)에서 삭제가 일어나느 선형 자료구조

![queue](https://wikidocs.net/images/page/198491/queue.png)

### 큐의 ADT
* 속성:
  * `queue`: 원소 저장 리스트
  * `front`: 제일 처음 들오온 데이터를 가리킴
  * `rear`: 마지막으로 들어온 데이터를 가리킴
* 주요 연산:
  * void `enQueue()`: 큐의 뒤쪽(rear)에 원소 삽입
  * type `deQueue()`: 큐의 앞쪽(front) 원소 삭제 및 반환
  * type `peek()` / `front()`: 큐의 앞쪽 원소 확인(삭제하지 않음)
  * boolean `isEmpty()`: 비어있는지 확인
  * int `size()`: 큐의 크기 반환

### 시간 복잡도
* enqueue (deque 사용): `O(1)`
* dequeue (deque 사용): `O(1)`
* peek: `O(1)`
* isEmpty: `O(1)`
* **주의**: 리스트의 pop(0)은 `O(n)` - 따라서 deque 사용 권장

### 리스트로 구현 (비효율적)

```python
class Queue:
  def __init__(self):
    self.items = [] # 리스트로 초기화

  def enqueue(self, item):
    self.items.append(item)
    print(f'{item} 삽입 완료')

  def dequeue(self):
    if len(self.item) == 0:
      print("큐가 비어있습니다.")
      return
    else:
      return self.items.pop(0)

  def is_empty(self):
    return len(self.items) == 0
    
  def size(self):
    return len(self.items)
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
#### 리스트 구현의 단점
* 리스트를 이용하면 삭제 후 공간을 재사용할 수 없고 속도가 느리다.
* 이를 해결하기 위해 `deque` 사용.

## 덱(Deque, Double-ended Queue)
* 특징: **양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조**
* 스택 + 큐의 기능을 모두 가진 자료구조
* **Deque = Double Ended Queue**의 약자

### 덱의 특징
* 앞쪽(front)과 뒤쪽(rear) 양쪽에서 삽입/삭제 가능
* 스택처럼 사용 가능 (한쪽만 사용)
* 큐처럼 사용 가능 (양쪽 사용)
* 더 유연하지만 메모리를 더 사용

### 덱의 주요 연산
* `append(item)`: 오른쪽 끝에 삽입
* `appendleft(item)`: 왼쪽 끝에 삽입
* `pop()`: 오른쪽 끝 삭제 및 반환
* `popleft()`: 왼쪽 끝 삭제 및 반환
* `extend(iterable)`: 오른쪽에 여러 원소 추가
* `extendleft(iterable)`: 왼쪽에 여러 원소 추가
* `rotate(n)`: n만큼 회전 (양수: 오른쪽, 음수: 왼쪽)

### 시간 복잡도
* append/appendleft: `O(1)`
* pop/popleft: `O(1)`
* 중간 삽입/삭제: `O(n)`
* 인덱스 접근: `O(n)`

### deque로 구현(권장)

```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque() # deque로 초기화
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        if self.is_empty():
            raise Exception("Queue is empty")
        return self.items.popleft()
    
    def peek(self):
        if self.is_empty():
            raise Exception("Queue is empty")
        return self.items[0]
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```
### deque로 스택 구현

```python
from collections import deque

# 스택으로 사용 (오른쪽만 사용)
stack = deque()
stack.append(1)    # push
stack.append(2)
stack.append(3)
print(stack.pop()) # 3
```
### deque로 큐 구현

```python
from collections import deque

# 큐로 사용 (오른쪽 삽입, 왼쪽 삭제)
queue = deque()
queue.append(1)        # enqueue
queue.append(2)
queue.append(3)
print(queue.popleft()) # 1 (dequeue)
```

### 활용 예시 - 카드 뽑기 문제
두 카드 뭉치에서 순서대로 카드를 뽑아 goal 문자열을 만들 수 있는지 확인

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

  return 'Yes' if not goal else 'No'

# 테스트
print(solution(["i", "drink", "water"], ["want", "to"], 
               ["i", "want", "to", "drink", "water"]))  # Yes
```

### 활용 예시 - BFS(너비 우선 탐색)
그래프에서 BFS 탐색

```python
from collections import deque

def bfs(graph, start):
  visited = set()  # 방문한 곳 중복 제거
  queue = deque([start]) # start로 초기화
  visited.add(start)     # start값을 방문하고 시작
  result = []

  while queue:
    node = queue.popleft()        # 'A' # ['B'] # ['C'] # ['D'] # ['E'] # ['F']
    result.append(node)           # ['A'] # ['A', 'B']  # ['A', 'B', 'C'] # ['A', 'B', 'C', 'D'] # ['A', 'B', 'C', 'D', 'E'] # ['A', 'B', 'C', 'D', 'E', 'F']

    for neighbor in graph[node]:  # ['B', 'C'] # ['A', 'D', 'E']  # ['A', 'F'] # ['B'] # ['B', 'F'] # ['C', 'E']
      if neighbor not in visited: 
        visited.add(neighbor)     # ['A', 'B', 'C'] # ['A', 'B', 'C', 'D', 'E'] # ['A', 'B', 'C', 'D', 'F']
        queue.append(neighbor)    # ['B', 'C'] # ['C', 'D', 'E']  # ['D', 'E', 'F']

  return result # ['A', 'B', 'C', 'D', 'E', 'F']

# 테스트
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}
print(bfs(graph, 'A')) # ['A', 'B', 'C', 'D', 'E', 'F']
```

## 스택 vs 큐 비교

| 구분 | 스택 (Stack) | 큐 (Queue) |
|------|-------------|-----------|
| **구조** | `LIFO` (Last In First Out) | `FIFO` (First In First Out) |
| **삽입 위치** | `top` (한쪽 끝) | `rear` (뒤쪽) |
| **삭제 위치** | `top` (한쪽 끝) | `front` (앞쪽) |
| **주요 연산** | `push`, `pop`, `peek` | `enqueue`, `dequeue`, `peek` |
| **활용** | 함수 호출, DFS, 괄호 검사, Undo | 프로세스 스케줄링, BFS, 캐시 |
| **파이썬 구현** | `list` | `collections.deque` |

### 언제 무엇을 사용할까?
* **스택**: 가장 최근 것이 중요한 경우 (되돌리기, 최근 기록, DFS)
* **큐**: 순서가 중요한 경우 (대기열, 순서 처리, BFS)