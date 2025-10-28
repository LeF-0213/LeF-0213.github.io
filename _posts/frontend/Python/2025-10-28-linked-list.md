---
layout: post
related_posts:
    - /frontend/python
title:  "연결 리스트(Linked List) 정리(단일 vs 양방향)"
date:   2025-10-28
categories:
  - frontend
  - python
description: >
  단일 연결 리스트(Singly Linked List)와 양방향 연결 리스트(Doubly Linked List)에 대한 개념 정리
---
* toc
{:toc .large-only}

# 연결 리스트(Linked List)
![linkedList](https://velog.velcdn.com/images%2F717lumos%2Fpost%2F260ef0bf-ba0a-4f9f-bea7-8f6f984509a1%2F%EB%A6%AC%EC%8A%A4%ED%8A%B8.png)
연결 리스트는 하나의 객체를 이루는 노드가 연결되어 리스트를 이루는 자료구조이다.

## 연결 리스트의 특징
* 노드에는 값을 담고 있는 **'데이터'**와 다음 노드를 가리키는 **'링크'**정보를 저장하고 있다.
* **데이터**에는 숫자, 문자열, 또다른 연결리스트 등 다양한 형식을 가질 수 있다.
* 일반적으로 리스트의 맨 앞 노드를 **헤드(Head)**, 맨 마지막 노드를 **테일(Tail)**이라고 한다.
* 배열과 달리 메모리 공간이 연속적이지 않아도 되며, 동적으로 크기 조절이 가능하다.(파이썬은 배열도 동적)

# 단일 연결 리스트(Singly)와 이중 연결 리스트(양방향, Doubly)
![SingleVsDouble](https://velog.velcdn.com/images%2F717lumos%2Fpost%2Fb38d2c1e-7878-4b17-93b1-07cc0b99096a%2F%EB%A6%AC%EC%8A%A4%ED%8A%B82.png)

## 단일 연결 리스트(Singly Linked List)
### 구조
* 각 노드가 **데이터**와 **다음 노드를 가리키는 포인터(`next`)**로 구성
* 한 방향으로만 순회 가능(head -> tail)
* **마지막 노드의 next**는 **null**을 가리킨다.

### 노드 구조 예시

```python
class Node:
  def __init__(self, data):
    self.data = data    # 데이터 저장
    self.next = None    # 다음 노드를 가리키는 포인터
```
### 주요 연산
#### 삽입(Insert)
**맨 앞에 삽입 - O(1)**

```python
def insert_front(head, data):
  new_node = Node(data)
  new_node.next = head
  return new_node     # 새로운 head 반환
```
**맨 뒤에 삽입 - O(n)**

```python
def insert_end(head, data):
  new_node = Node(data)
  if head is None:
    return new_node

  current = head
  while current.next: 마지막 노드까지 탐색
    current = current.next
  current.next = new_node
  return head
```
**중간에 삽입 - O(n)**
![중간 삽입 이미지](https://velog.velcdn.com/images%2F717lumos%2Fpost%2F327827ab-8838-4b57-9ebc-4e98287793e5%2F%EA%B7%B8%EB%A6%BC6.png)

```python
def insert_after(prev_node, data):
  if prev_node is None:
    return
  new_node = Node(data)
  new_node.next = prev_node.next
  prev_node.next = new_node
```

#### 삭제(Delete)
**맨 앞 삭제 - O(1)**

```python
def delete_front(head):
  if head is None:
    return None
  # 첫 노드가 바뀌기 때문에 head.next 반환
  return head.next
```
**특정 값 삭제 - O(n)**
![특정 값 삭제 이미지](https://velog.velcdn.com/images%2F717lumos%2Fpost%2Faec7c7ad-700c-45ac-b1fc-4776a76a3059%2F%EA%B7%B8%EB%A6%BC7.png)

```python
def delete_value(head, value):
  if head is None:
    return None

  # 헤드 노드가 삭제 대상인 경우(맨 앞 삭제)
  if head.data == value:
    return head.next

  # current.next.data == value일 때 while문이 중단됨 
  # 결국 이전 노드를 찾는 과정
  current = head
  while current.next and current.next.data != value:
    current = current.next

  # 이전노드와 current.next.next를 연결
  # 결국 연결이 사라진 current.next는 가비지 컬렉션에 의해 삭제
  if current.next:
    current.next = current.next.next

  # head는 바뀌지 않았기 때문에 head 반환
  return head
```

#### 탐색(Search) - O(n)

```python
def search(head, value):
  current = head
  # current.data == value일 때까지 while문 동작
  while current:
    if current.data == value:
      return current
    current = current.next
  # 발견하기 못하면 None 반환
  return None
```

### 장단점

| 단일 연결 리스트 장점 | 단일 연결 리스트 단점 |
|---------------------|---------------------|
| • 메모리 효율적 (노드당 포인터 1개만 필요) | • 역방향 탐색 불가능 |
| • 구현이 비교적 간단함 | • 이전 노드 접근 시 O(n) 시간 소요 |
| • 삽입/삭제 시 포인터 업데이트가 적음 (next만) | • 특정 노드 삭제 시 이전 노드를 찾아야 함 |
| • 순방향 탐색이 빠름 | • 임의 접근(random access) 불가 - O(n) |
| • 배열보다 동적 크기 조절이 용이함 | • 양방향 순회가 필요한 경우 비효율적 |

## 양방향 연결 리스트

### 구조
* 각 노드가 **데이터**, **이전 노드 포인터(`prev`)**와 **다음 노드 포인터(`next`)**로 구성
* 양방향 순회 가능(head <-> tail)
* **양쪽 끝이 모두 null**로 연결된다.

### 노드 구조 예시
![최초 노드 삽입](https://user-images.githubusercontent.com/81945553/175527957-3d7b9598-d844-4a2e-8f57-7336f3201959.png)

```python
class DoublyNode:
  def __init__(self, data):
    self.data = data  # 데이터 저장
    self.prev = None  # 이전 노드를 가리키는 포인터
    self.next = None  # 다음 노드를 가리키는 포인터
```

### 주요 연산
#### 삽입(Insert)
**맨 앞에 삽입 - O(1)**
![맨 앞 삽입](https://user-images.githubusercontent.com/81945553/175528143-179b3a44-7d3b-4aca-a786-a9a9e51de382.png)

```python
def insert_front(head, data):
  new_node = DoublyNode(data) # prev=None, next=None
  new_node.next = head  # 만약 head가 None일 경우, None
  if head:              # 때문에 값이 없다면 head.prev가 존재하지 않기에 False
    head.prev = new_node  # 값이 있다면 맨 앞 요소의 prev는 new_node가 됨
  return new_node
```
**맨 뒤에 삽입 - O(1)** (tail 포인터가 있는 경우)
![맨 뒤 삽입](https://user-images.githubusercontent.com/81945553/175528670-12614ba8-7033-488b-b72e-a2b7fd4b44d8.png)

```python
def insert_end(tail, data):
  new_node = DoublyNode(data)
  if tail:  # 빈 리스트인지 확인
    tail.next = new_node
    new_node.prev = tail
  return new_node
```
**특정 노드 뒤에 삽입 - O(1)**
![맨 앞 삽입](https://user-images.githubusercontent.com/81945553/175528839-8f2c3332-b28f-4986-ba69-cf8c86979a2a.png)

```python
def insert_after(prev_node, data):
  if prev_node is None:
    return

  # 새노드의 prev랑 next를 지정
  new_node = DoublyNode(data)
  new_node.next = prev_node.next
  new_node.prev = prev_node

  # 양 옆에 노드에 새 노드를 연결
  if prev_node.next: # next != None, 끝이 아닐 경우
    prev_node.next.prev = new_node # prev_node.next 노드의 prev가 new_node가 됨
  prev_node.next = new_node
```
#### 삭제(Delete)
**노드가 하나일 때 삭제**
![하나 삭제](https://user-images.githubusercontent.com/81945553/175529004-1a86cd72-c3c6-44ca-a65f-1eefb7cf3242.png)

**맨 앞 노드 삭제**
![맨 앞 삭제](https://user-images.githubusercontent.com/81945553/175529055-735184d5-e121-4b45-868f-8d7293d569a7.png)

**맨 뒤 노드 삭제**
![맨 뒤 삭제](https://user-images.githubusercontent.com/81945553/175529120-982ee5a0-c49e-423f-94ca-37aef0d1f0b2.png)

**특정 노드 삭제** (노드를 알고 있을 때)
![특정 노드 삭제](https://user-images.githubusercontent.com/81945553/175529200-72c98f9a-f301-4f3c-83d7-8ad6f9c33d89.png)

```python
def delete_node(node):
  if node is None:
    return

  # 이전 노드의 next 업데이트
  if node.prev:
    node.prev.next = node.next

  # 다음 노드의 prev 업데이트
  if node.next:
    node.next.prev = node.prev
```
#### 역방향 순회
이중 연결 리스트는 역방향 순회가 가능하다.
![SingleVsDouble](https://velog.velcdn.com/images%2F717lumos%2Fpost%2Fb38d2c1e-7878-4b17-93b1-07cc0b99096a%2F%EB%A6%AC%EC%8A%A4%ED%8A%B82.png)

```python
def print_reverse(tail):
  current = tail
  while current:
    current = current.prev
```

### 장단점

| 양방향 연결 리스트 장점 | 양방향 연결 리스트 단점 |
|---------------------|---------------------|
| • 양방향 탐색 가능 (앞↔뒤) | • 메모리 사용량 증가 (노드당 포인터 2개) |
| • 특정 노드에서 이전 노드 접근이 O(1) | • 구현이 복잡함 (포인터 관리 2배) |
| • 노드 삭제가 효율적 (이전 노드 탐색 불필요) | • 삽입/삭제 시 더 많은 포인터 업데이트 필요 |
| • 역순 순회가 용이함 | • 단일 연결 리스트보다 공간 복잡도 높음 |
| • 양쪽 끝(head/tail)에서 삽입/삭제 최적화 가능 | • 캐시 효율성이 상대적으로 낮음 |

## 종합 비교표

| 비교 항목 | 단일 연결 리스트 | 양방향 연결 리스트 |
|----------|-----------------|-------------------|
| 메모리 사용 | 적음 (포인터 1개) | 많음 (포인터 2개) |
| 구현 복잡도 | 낮음 | 높음 |
| 순방향 탐색 | O(n) | O(n) |
| 역방향 탐색 | 불가능 | O(n) |
| 이전 노드 접근 | O(n) | O(1) |
| 삽입 연산 | O(1)* | O(1)* |
| 삭제 연산 | O(n)** | O(1)* |
| 포인터 업데이트 | 1개 (next) | 2개 (prev, next) |
| 공간 복잡도 | O(n) | O(2n) |
| 적합한 용도 | 스택, 큐, 메모리 제한적 환경 | LRU 캐시, 브라우저 히스토리, Undo/Redo |

\* 삽입/삭제할 위치를 알고 있을 때  
\** 삭제할 노드만 알고 있을 때 (이전 노드 탐색 필요)

## 엣지 케이스 처리
* 빈 리스트 (head가 None인 경우)
* 노드가 1개인 리스트
* 헤드 또는 테일 노드를 삭제하는 경우

## 더미 노드(Dummy Node) 활용
엣지 케이스를 단순화하기 위해 실제 데이터를 갖지 않는 **더미 헤드/테일 노드**를 사용할 수 있다.

```python
class DoublyLinkedList:
  def __init__(self):
    self.head = DoublyNode(None)  # 더미 헤드
    self.head = DoublyNode(None)  # 더미 테일
    self.head.next = self.tail
    self.tail.prev = self.head
```

## 실제 사용 사례

### 단일 연결 리스트
- **스택(Stack) 구현**: push/pop이 head에서만 일어남
- **큐(Queue) 구현**: enqueue는 tail, dequeue는 head
- **메모리 제한적인 임베디드 시스템**
- **순방향 반복자가 필요한 경우**
- **해시 테이블의 체이닝(Chaining)**

### 양방향 연결 리스트
- **LRU 캐시**: 최근 사용 항목을 앞으로 이동
- **브라우저의 앞으로/뒤로 가기**
- **음악 플레이어의 재생목록**
- **텍스트 에디터의 Undo/Redo**
- **덱(Deque) 구현**: 양쪽 끝에서 삽입/삭제
