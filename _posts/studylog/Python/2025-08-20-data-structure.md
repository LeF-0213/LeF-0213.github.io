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

1. 원소 집어넣기 push()
stack = []
def push(self, x):
  self.스택이름.append(x)

2. 원소 삭제 pop()
def pop(self):
  self.스택이름.pop()

3. 
