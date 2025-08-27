---
layout: post
related_posts:
    - /studylog/python
title:  "자료구조"
date:   2025-08-05
categories:
  - studylog
  - python
description: >
  
---
* toc
{:toc .large-only}

Python 3(자료구조 + 알고리즘(DFS, BFS, 그리드 알고리즘) 구현 방법 설명

# 자료구조(Data Structure)란?
자료구조 => 데이터의 구조(저장) ==> 데이터를 탐색하기 위한 목적(빨리 찾기 위한 방법)
기본 자료구조, 확장형 자료 구조

## 선형 자료구조
**데이터의 저장방식**: 데이터를 정해진 순서대로 저장 및 탐색
- 배열 
- 연결리스트 
- 스택 
- 큐


## 비선형 자료구조
**데이터의 저장방식**: 데이터를 저장하는 순서가 정해져있지 않다. 
- 트리 
- 그래프

모든 자료구조의 기본은 배열이라고 생각하면 된다.
- 배열로 스택 구현
- 배열로 큐 구현
- 배열로 트리 및 그래프 구현
=> 탐색 결과를 배열로 나타낸다.(파이썬에서는 배열이라는 이름 대신에 --> 리스트 사용)

배열(array, list)
가장 빠르게 탐색 ==> list

Python에서 리스트(배열)을 구현하는 방법
a = [10, 20, 30, 40]
데이터를 삽입:append(), insert(), input(), put() => ADT(추상형 데이터)
검사 항목: 데이터가 들어갈 자리가 있는지 => isFull()
삭제할 데이터가 있는지 => isEmpty()

연결 리스트(Linked List)
배열 리스트 구현보다 힘들다
Node(데이터의 저장 장소, 오직 하나의 데이터만을 저장)
**장점**: 무한대로 늘어남
**단점**: 비효율적이다.(느림)

## stack(FILO)
데이터 삭제 => pop()
현재 데이터를 가리키는 변수 => top
(마지막으로 들어온 데이터를 가리킨다)

## Queue(FIFO)
enQueue
rear, front

deQueue

## Tree(이진 트리)
루트 노드
노드
간선
단말 노드(자식이 없는 노드)

List의 객체 구조
파이썬에서는 리스트 자료구조가 기본적으로 제공(내부에서 어떻게 돌아가는지 알 필요(X))
ADT 리스트
inser(), append(), pop(), remove(), index(), clear(), count(), extend(), copy(), reverse(), sort()

num = [1, 3, 5, 7]

## 원소 삽입
### insert()
리스트의 인덱스 번호에 데이터를 삽입
```python
num.insert(1, 8)
num
# [1, 8, 3, 5, 7]
```

### append()
리스트의 마지막에 데이터를 삽입
```python
num.append(8)
num
# [1, 3, 5, 7, 8]
```

## 원소 삭제
### pop()
리스트와 스택에 따라 사용이 달라짐
```python
num.pop(3)
num
# [1, 3, 7]
```

```python
num.pop()
num
# [1, 3, 5]
```

```python
num.pop(-1)
num
# [1, 3, 5]
```

### remove()
삭제할 원소를 직접적으로 선택
```python
num.remove(5)
num
# [1, 3, 7]
```

## 리스트 비우기
### clear()
```python
num.clear()
num
# []
```

리스트, 큐, 스택 ==> 반드시 빈 리스트, 큐, 스택을 생성

연결 리스트 
배열의 공간 낭비를 피할 수 있는 자료구조    
원소를 추가할 때마다 공간을 할당받아 추가하는 동적 할당 방식    
원소를 저장하는 최소 단위 = 노드

data, item | 포인터(c언어), reference(java)

마지막 노드라고 가정하면 더 이상 노드가 없다라는 의미로 None를 할당

head => 연결 리스트에서 첫번째 노드에 대한 레퍼런스
numItems => 연결 리스트에 들어있는 원소의 총 수

newNode.next = pre.next
pre.next = newNode

```python
if i == 0:
  newNode.item = x
  newNode.next = head
  head = newNode
  numItem == 1
else:
  newNode.tiem = x
  newNode.next = pre.next
  pre.next = newNode
  numItem += 1
```

```python
class ListNode:
  def __init__(self, newItem, nextNode):
    self.item = newItem
    self.next = nextNode
```

```python
class LinkedListBasic:
	def __init__(self):
		self.__head = ListNode('dummy', None)
		self.__numItems = 0

	def insert(self, i:int, newItem):
		if i >= 0 and i <= self.__numItems: # 노드의 범위
			prev = self.__getNode(i - 1)  # 앞선 노드
			newNode = ListNode(newItem, prev.next) # 새로운 노드의 item값과, 연결된 주소
			prev.next = newNode # prev의 다음 노드로 위치
			self.__numItems += 1 # 추가된 노드 수 더하기
		else:
			print("index", i, ": out of bound in insert()") 
 
	def append(self, newItem):
		prev = self.__getNode(self.__numItems - 1)  # 끝에서 앞인 노드
		newNode = ListNode(newItem, prev.next)
		prev.next = newNode
		self.__numItems += 1


	def pop(self, i:int):  
		if (i >= 0 and i <= self.__numItems-1): 
			prev = self.__getNode(i - 1)
			curr = prev.next # 주소값을 prev 다음으로 위치
			prev.next = curr.next # 원래 prev 다음 값을 현재 주소값의 다음으로 위치
			retItem = curr.item
			self.__numItems -= 1
			return retItem
		else:
			return None
	
	def remove(self, x):
		(prev, curr) = self.__findNode(x)
		if curr != None:
			prev.next = curr.next
			self.__numItems -= 1
			return x
		else:
			return None

	def get(self, i:int):
		if self.isEmpty():
			return None
		if (i >= 0 and i <= self.__numItems - 1):
			return self.__getNode(i).item
		else:
			return None
 
	def index(self, x) -> int:
		curr = self.__head.next	 
		for index in range(self.__numItems):
			if curr.item == x:
				return index
			else:
				curr = curr.next
		return -2 # 

	def isEmpty(self) -> bool:
		return self.__numItems == 0

	def size(self) -> int:
		return self.__numItems

	def clear(self):
		self.__head = ListNode("dummy", None)
		self.__numItems = 0

	def count(self, x) -> int:
		cnt = 0
		curr = self.__head.next  
		while curr != None:
			if curr.item == x:
					cnt += 1
			curr = curr.next
		return cnt

	def extend(self, a): 
		for index in range(a.size()):
			self.append(a.get(index))
 
	def copy(self):
		a = LinkedListBasic()
		for index in range(self.__numItems):
			a.append(self.get(index))
		return a

	def reverse(self):
		a = LinkedListBasic()
		for index in range(self.__numItems):
			a.insert(0, self.get(index))
		self.clear()
		for index in range(a.size()):
			self.append(a.get(index))

	def sort(self) -> None:
		a = []
		for index in range(self.__numItems):
			a.append(self.get(index))
		a.sort()
		self.clear()
		for index in range(len(a)):
			self.append(a[index])
 
	def __findNode(self, x) -> (ListNode, ListNode):
		prev = self.__head  # 더미 헤드
		curr = prev.next    # 0번 노드
		while curr != None:
			if curr.item == x:
				return (prev, curr)
			else:
				prev = curr; curr = curr.next
		return (None, None)

	def __getNode(self, i:int) -> ListNode:
		curr = self.__head # 더미 헤드, index: -1
		for index in range(i+1):
			curr = curr.next
		return curr

	def printList(self):
		curr = self.__head.next 
		while curr != None:
			print(curr.item, end = ' ')
			curr = curr.next
		print()
```

## 스택(FIFO, LIFO)
push(스택의 이름, 원소) 데이터를 삽입, push(stack, 'A')
pop() 데이터를 삭제(마지막으로 들어온 데이터를 삭제), pop(stack)
top => 마지막으로 들어온 데이터, 원소가 들어오기 전 먼저 증가

원소 삽입, 삭제 코드 작성

1. 빈 스택 구현
스택 구현 => 자료구조나 알고리즘을 파악하는 것이 중요
```python
stack = [] # 스택의 리스트 선언
max_size = 10 # 배열의 크기

# 스택이 가득 찾는지 확인하는 함수
def isFull(stack):
  return max_size == len(stack)

# 스택이 비어있는지 확인하는 함수
def isEmpty(stack):
  return max_size == 0
```

2. 원소 집어넣기 push()
stack = []
def push(self, x):
  self.스택이름.append(x)
```python
def push(stack, item):
  if isFull(stack):
    print("더 이상 원소를 집어 넣을 수 없군요")
  else:
    stack.append(item)
    print("원소 추가 완료")
```

1. 원소 삭제 pop()
def pop(self):
  self.스택이름.pop()

```python
def pop(stack):
  if isEmpty(stack):
    print("비어있습니다.")
  else:
    return stack.pop()
```

파이썬의 리스트는 정적배열을 관리하지 않고 --> 동적 배열을 이용한다.
위에서 선언한 max_size, isFull() 하나의 변수와 하나의 함수는 사용하지 않는다.

```python
stack = [] # 스택의 리스트 선언

def push(stack, item):
  stack.append(item)
  print("원소 추가완료")

def pop(stack):
  if len(stack) == 0:
    print("비어 있습니다")
    return None
  else:
    return stack.pop()
```

```python
stack = []

stack.append(1)
stack.append(2)
stack.append(3)

stack.pop()

stack_size = len(stack)
```

문제] 스택을 이용하여 문제를 해결할 수 있다
소괄호 '(' 와 ')' 구성된 문자열이 존재한다.
문자열의 이름은 str이다.
이때 괄호의 짝이 맞는지 판별하는 함수를 작성하는 문제이다.
단, 조건은 반드시 열린 괄호가 먼저 나와야 한다.

예] 
((()))()
((()))) -> )
짝이 맞으면 결과값을 True / 아니면 False

```python
def isParentheses(str):
  stack = []

  for i in str:
    if i == '(':
      stack.append(i)
    else:
      if not stack:
        return false
      else:
        stack.pop()
    
    return len(stack) == 0
```

문제2] 알파벳 소문자로 구성된 문자열에서 같은 알파벳이 2개 붙어있는 짝을 찾는다.
  짝을 찾은 다음에는 그 둘을 제거 한 뒤 앞뒤의 문자열을 이어붙인다.
  이러한 과정을 반복하면 문자열을 모두 제거하면 종료가 된다
  제거가 완전히 다 되면 1을 반환하고 그렇지 않으면 0을 반환

  예] baabaa -> bbaa -> aa
    cdcd -> 0

```python
def alpabet(str):
    stack = []
    
    for i in str:
        if not stack or stack[-1] != i:
            stack.append(i)
        else:
            stack.pop()

    return int(not stack)
```

## Queue(FIFO)
줄을 거는 개념, stack(FILO)과 거의 비슷하다.
결과값을 => 리스트(append)
deque <- <- enque

front
제일 처음으로 들어온 데이터를 가라킨다
rear
마지막으로 들어온 데이터를 가리킨다

front, rear = -1
처음의 초기값으로 front / rear => -1

Queue/Stack
이 두 개의 선형 리스트는 같은 함수를 이용하여   
데이터를 추가/삭제한다.
push(), pop(), append()

Queue는 삭제된 공간을 다시 재사용할 수 없다.

queue = [] # 빈 큐를 생성

# 큐에 데이터 추가
queue.append(1)
queue.append(2)
queue.append(3)

item = queue.pop()
print(item)

삭제된 공간을 사용할 수 없다는 단점을 보완하기 위해
큐에서는 `덱`을 활용한다.

덱을 사용하려면 라이버리를 삽입
from collections import deque

queue = deque()

queue = append(1)
queue = append(2)
queue = append(3)

item = queue.popleft() # 기본 삭제 방향
print(item)

pop() / popleft() 데이터를 삭제하는 속도


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

```python
import math

def solution(progresses, speeds):
  answer = []
  n = len(progresses)

  days_left = [math.ceil((100 - progresses[i]) / speeds[i]) for i in rage(n)]

  count = 0
  max_day = days_left[0]

  for i in range(n):
    if days_left[i] <= max_day:
      count += 1
    else:
      answer.append(count)
      count = 1
      max_day = days_left[i]
  
  answer.append(count)
  return answer
```

## Tree
데이터를 저장하고 탐색하기에 가장 유용한 구조를 가지고 있다.

노드 
root 
간선
차수 => 노드의 수
단말 노드(리프 노드) => 더 이상의 자식 노드가 없는 노드

일반적으로 트리라고 이야기 하는 것은 => 거의 99.9% 이진트리를 사용한다.
이진 트리로 되어있는 것을 배열로 표현하려고 하면 인덱스는 0이 아니라 1부터 시작(루트 노드)
루트를 기준으로 왼쪽 노드부터 우선

부모노드의 배열 인덱스 값 * 2
부모 노드의 배열 인덱스 값 * 2 + 1

트리의 순회 방향
1. 전위 순회(preorder)
현재 노드를 부모로 생각했을 때 부모 노드 -> 왼쪽 노드 -> 오른쪽 노드
2. 중위 순회(inorder)
현재 노드를 부모로 생각했을 때 왼쪽 노드 -> 부모 노드 -> 오른쪽 노드
3. 후위 순회(postorder)
현재 노드를 부모로 생각했을 대 왼쪽 노드 -> 오른쪽 노드 -> 왼쪽 노드

이진 트리에서 가장 중요한 점은 탐색을 효율적으로 할 수 있도록 트리를 구축하는 방법
(데이터를 잘 정리하면 탐색이 빠르고 쉽다)
예를 들어 데이터가 3 -> 4 -> 2 -> 8 -> 9 -> 7 -> 1

이진 탐색 트리

기준 => root
종료 기준 => 찾고자하는 데이터를 찾으면 종료

작으면 왼쪽 크면 오른쪽

평양 이진 트리 
균형 이진 트리 => 레드-블랙, AVL 트리

그래프, 트리 => 비선형 자료구조 공통점이다.
그래프 => 순회, 트리 => 순회(X)

방향 그래프, 무방향 그래프(양방향)
노드(vertex), 간선(Edge), 가중치

데이터를 담고 있는 저장소 -> 노드(정점)
노드를 잇는 것 -> 간선(Edge)
간선의 방향 -> 무방향(양방향), 단방향
노드와 노드 사이의 값 -> 가중치

그래프의 구현 방식
1. 인접 행렬(2차원 배열)
2. 인접 리스트(linked)

그래프 탐색
깊이 우선 탐색
더 이상 탐색 할 노드가 없을 때까지 탐색(일단 한 방향으로)
너비 우선 탐색
현재 노드를 기준으로 가장 가까운 노드부터 모두 방문하고 다음 노드로 이동

깊이 우선 탐색(DFS) => 스택 방식에 의해 구현
너비 우선 탐색(BFS) => 큐 방식에 의해 구현
완전 탐색(모든 노드를 방문)

## DFS(깊이 우선 탐색)
탐색 시작 노드를 스택에 삽입하고 방문 처리 한다.
스택의 최상단 노드 중 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 삽입하고 방문 처리
2번의 과정을 더  방문할 노드가 없을 때까지 반복

1. 스택을 생성한다.(빈 스택)
stack = []
노드 중에서 가장 작은 데이터를 잡는다.
2. 방문 노드를 기준으로 방문하지 않는 인접 노드를 탐색
(그 중 노드 값이 작은 노드를 스택에 삽입 후 방문 처리)
3. 마지막으로 방문한 노드를 기준
인접 노드 중 방문하지 않은 노드를 스택에 삽입하고 방문 처리

위의 방식으로 탐색 한 순서는
1 -> 2 -> 7 -> 6 -> 8 -> 3 -> 4 -> 5

```python
def dfs(graph, v, visited):
  visited[v] = True     # 현재 노드 v를 방문 처리
  print(v, end = '')    # 방문한 노드 출력

  for i in graph[v]:    # 현재 노드 v와 연결된 노드들을 확인
    if not visited[i]:  # 아직 방문하지 않았다면
      dfs(graph, i, visited)  # 재귀적으로 방문
```
매개변수 의미
* **graph**: 인접 리스트
* **V**: 현재 방문할 노드 번호
* **visited**: 방문 여부를 저장하는 리스트

## BFS(너비 우선 탐색)
가까운 노드부터 탐색하는 알고리즘

```python
from collections import deque

def bfs(graph, start, visited):
  queue = deque([start])
  visited[start] = True

  while queue:
    v = queue.popleft()
    print(v, end = '')
    for i in graph[v]:
      if not visited[i]:
        queue.append(i)
        visited[i] = True
```

정수 배열 num이 주어진다. 이 num에서 서로 다른 인덱스에 있는 2개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 반환하는 함수를 작성하시오(함수 이름은 solution()이다.) (중복 값은 허용하지 않는다.)

num = [2, 1, 3, 4, 1]
num = [5, 0, 2, 7]

```python
def sol(num):
  result = []

  for i in range(len(num)):
    for j in range(i + 1, len(num)):
      result.append(num[i] + num[j])
  
  result = sorted(set(result))
  return result
```

정렬 -> 모든 데이터를 나열하는 것을 의미(사용자가 정의한 순서대로)
-> 오른차순/ 내림차순
-> 데이터를 쉽게 탐색하기 위해서

1. 삽입 정렬 => 데이터의 전체 영역에서 정렬된 영역과 
