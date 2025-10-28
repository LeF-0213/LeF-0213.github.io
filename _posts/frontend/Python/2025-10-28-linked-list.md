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

## 배열 vs 연결 리스트

| 특성 | 배열(Array) | 연결 리스트(Linked List) |
|------|------------|------------------------|
| 메모리 할당 | 연속적 | 비연속적 |
| 크기 | 고정적 (정적) | 가변적 (동적) |
| 접근 시간 | O(1) - 인덱스로 직접 접근 | O(n) - 순차 탐색 필요 |
| 삽입/삭제 | O(n) - 요소 이동 필요 | O(1) - 포인터만 변경 |
| 메모리 효율 | 데이터만 저장 | 포인터 추가 저장 필요 |
| 캐시 효율성 | 높음 (연속 메모리) | 낮음 (분산 메모리) |

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
### 함수형 스타일 구현(개념 이해용)
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
**특정 노드 뒤에 삽입 - O(n)**
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

### 클래스 기반 구현(실제 구현)

```python
class Node:
  def __init__(self, data):
    self.data = data
    self.next = None

class SiglyLinkedList:
  def __init__(self):
    self.head = None
    self.size = 0

  def is_empty(self):
    return self.head is None

  def get_size(self):
    return self.size

  # 맨 앞에 삽입 - O(1)
  def insert_front(self, data):
    new_node = Node(data)
    new_node.next = self.head
    self.head = new_node
    self.size += 1

  # 맨 뒤에 삽입 - O(n)
  def insert_end(self, data):
    new_node = Node(data)

    if self.head is None:
      self.head = new_node
    else:
      current = self.head
      while current.next:
        current = current.next
      current.next = new_node

    self.size += 1

  # 특정 노드 뒤에 삽입 - O(1) (노드를 알고 있을 때)
  def insert_after(self, prev_node, data):
    if prev_node is None:
      return
    
    new_node = Node(data)
    new_node.next = prev.node.next
    prev_node.next = new_node
    self.size += 1

  # 맨 앞 노드 삭제 - O(1)
  def delete_front(self):
    if self.head is None:
      return None

    deleted_data = self.head.data
    self.head = self.head.next
    self.size -= 1
    return deleted_data

  # 특정 값을 가진 노드 삭제 - O(n)
  def delete_value(self, value):
    if self.head is None:
      return False

    # 헤드 노드가 삭제 대상인 경우
    if self.head.data == value:
      self.head = self.head.next
      self.size -= 1
      return True

    # 이전 노드 찾기
    current = self.head
    while current.next and current.next.data != value:
      current = current.next

    if current.next:
      current.next = current.next.next
      self.size -= 1
      return True
  
  # 탐색 - O(n)
  def search(self, value):
    current = self.head
    while current:
      if current.data == value:
        return current
      current = current.next
    return None

  # 순회 출력
  def print_list(self):
    current = self.head
    while current:
      print(current.data, end="->")
      current = current.next
    print("None")

# 결과
sll = SinglyLinkedList()
sll.insert_end(1)
sll.insert_end(2)
sll.insert_end(3)
sll.insert_front(0)
sll.print_list()  # 0 -> 1 -> 2 -> 3 -> None

sll.delete_front()  # 0 삭제
sll.delete_value(2)  # 2 삭제
sll.print_list()  # 1 -> 3 -> None
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

### 함수형 스타일 구현 (개념 이해용)
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
```python
def delete_one(head, tail):
  if node is None:
  return head, tail

  if head == tail:
    return None, None
```

**맨 앞 노드 삭제**
![맨 앞 삭제](https://user-images.githubusercontent.com/81945553/175529055-735184d5-e121-4b45-868f-8d7293d569a7.png)

```python
def delete_head(head, tail, node):
  if node is None:
    return head, tail

  if node == head:
    if head == tail:
      return None, None
    head = head.next
    head.prev = None
    return head, tail
```
**맨 뒤 노드 삭제**
![맨 뒤 삭제](https://user-images.githubusercontent.com/81945553/175529120-982ee5a0-c49e-423f-94ca-37aef0d1f0b2.png)

```python
def delete_tail(head, tail, node):
  if node is None:
    return head, tail

  if node == tail:
    tail = tail.prev
    tail.next = None
    return head, tail
```
**중간 노드 삭제**
![특정 노드 삭제](https://user-images.githubusercontent.com/81945553/175529200-72c98f9a-f301-4f3c-83d7-8ad6f9c33d89.png)

```python
def delete_node(head, tail, node):
  if node is None:
    return head, tail

  node.prev.next = node.next
  node.next.prev = node.prev
  return head,, tail
```
#### 역방향 순회
이중 연결 리스트는 역방향 순회가 가능하다.
![SingleVsDouble](https://velog.velcdn.com/images%2F717lumos%2Fpost%2Fb38d2c1e-7878-4b17-93b1-07cc0b99096a%2F%EB%A6%AC%EC%8A%A4%ED%8A%B82.png)

```python
def print_backward(tail):
  current = tail
  while current:
    print(current.data, end=" ")
    current = current.prev
  print("None")
```
### 클래스 기반 구현(실제 구현)

```python
class DoublyNode:
  def __init__(self, data):
    self.data = data
    self.prev = None
    self.next = None

class DoublyLinkedList:
  def __init__(self):
    self.head = None
    self.tail = None
    self.size = 0

  def is_empty(self):
    return self.head is None

  def get_size(self):
    return self.size

  # 맨 앞에 삽입 - O(1)
  def insert_front(self, data):
    new_node = DoublyNode(data)

    if self.head is None:
      self.head = new_node
      self.tail = new_node
    else:
      new_node.next = self.head
      self.head.prev = new_node
      self.head = new_node

    self.size += 1

  # 맨 뒤에 삽입 - O(1)
  def insert_end(self, data):
    new_node = DoublyNode(data)

    if self.tail is None:
      self.head = new_node
      self.tail = new_node
    else:
      new_node.prev = self.tail
      self.tail.next = new_node
      self.tail = new_node

    self.size += 1

  # 특정 노드 뒤에 삽입 - O(1)
  def insert_after(self, prev_node, data):
    if prev_node is None:
      return

    new_node = DoublyNode(data)
    new_node.next = prev_node.next
    new_node.prev = prev_node

    if prev_node.next:  # prev_node가 tail이 아닌 경우
      prev_node.next.prev = new_node
    else:
      self.tail = new_node

    prev_node.next = new_node
    self.size += 1

  # 맨 앞 노드 삭제 - O(1)
  def delete_front(self):
    if self.head is None:
      return None

    deleted_data = self.head.data

    if self.head == self.tail: # 노드가 1개만 있는 경우
      self.head = None
      self.tail = None
    else:
      self.head = self.head.next
      self.head.prev = None

    self.size -= 1
    return deleted_data

  # 맨 뒤 노드 삭제 - O(1)
  def delete_end(self):
    if self.tail is None:
      return None

    deleted_data = self.tail.data

    if self.head == self.tail:
      self.head = None
      self.tail = None
    else:
      self.tail = self.tail.prev
      self.tail.next = None

    self.size -= 1
    return deleted_data

  # 특정 노드 삭제 - O(1) (노드를 알고 있을 때)
  def delete_node(self, node):
    if node is None:
      return

    # 삭제할 노드가 head인 경우
    if node == self.head:
      self.delete_front()
      return

    # 삭제할 노드가 tail인 경우
    if node == self.tail:
      self.delete_end()
      return

    # 중간 노드 삭제
    node.prev.next = node.next
    node.next.prev = node.prev

    # 메모리 누수 방지
    node.prev = None
    node.next = None

    self.size -= 1

  # 특정 값을 가진 노드 삭제 - O(n)
  def delete_value(self, value):
    current = self.head

    while current:
      if current.data == value:
        self.delete_node(current)
        return True
      current = current.next

    return False

  # 탐색 - O(n)
  def search(self, value):
    current = self.head
    while current:
      if current.data == value:
        return current
      current = current.next
    return None

  # 정방향 순회
  def print_forward(self):
    current = self.head
    while current:
      print(crrent.data, end=" ")
      current = current.next
    print("None")

  # 역방향 순회
  def print_backward(self):
    current = self.tail
    while current:
        print(current.data, end="  ")
        current = current.prev
    print("None")

# 결과
dll = DoublyLinkedList()
dll.insert_end(1)
dll.insert_end(2)
dll.insert_end(3)
dll.insert_front(0)

dll.print_forward()   # 0  1  2  3  None
dll.print_backward()  # 3  2  1  0  None

dll.delete_front()    # 0 삭제
dll.delete_end()      # 3 삭제
dll.print_forward()   # 1  2  None
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

### 시간 복잡도 조건 설명

**\* 삽입/삭제할 위치를 알고 있을 때 - O(1)**
- 이미 **특정 노드의 포인터**를 가지고 있는 경우
- 그 노드 바로 다음에 삽입하거나, 그 노드를 삭제할 때
- 포인터만 바꾸면 되므로 O(1) 시간 소요
```python
# 위치를 알고 있는 경우 - O(1)
current_node.next = new_node  # 바로 삽입 가능
```

**\*\* 삭제할 노드만 알고 있을 때 (이전 노드 탐색 필요) - O(n)**
- **단일 연결 리스트**의 경우
- 삭제할 노드는 알지만, **이전 노드의 포인터를 끊어야** 하는데
- 이전 노드를 찾으려면 처음부터 탐색해야 해서 O(n) 시간 소요
```python
# 단일 연결 리스트에서 노드 삭제
# delete_node를 삭제하려면
# 1. head부터 시작해서 delete_node의 이전 노드를 찾아야 함 - O(n)
# 2. prev_node.next = delete_node.next로 연결 끊기 - O(1)
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

## 순환 연결 리스트(Circular Linked List)
마지막 노드가 null 대신 첫 번째 노드를 가리키는 변형입니다.

### 단일 순환 연결 리스트

```python
# 마지막 노드
tail.next = head  # null 대신 head를 가리킴
```
### 양방향 순환 연결 리스트

```python
# 마지막 노드와 첫 노드 연결
tail.next = head
head.prev = tail
```
### 사용 사례
- 라운드 로빈 스케줄링
- 멀티플레이어 게임의 턴 관리
- 순환 버퍼 구현

## 주의사항 및 팁
### 메모리 누수 방지

```python
# 노드 삭제 시 명시적으로 참조 해제
def delete_node(self, node):
    if node.prev:
        node.prev.next = node.next
    if node.next:
        node.next.prev = node.prev
    
    # 메모리 누수 방지
    node.prev = None
    node.next = None
```

### 엣지 케이스 처리
* 빈 리스트 (head가 None인 경우)
* 노드가 1개인 리스트
* 헤드 또는 테일 노드를 삭제하는 경우

### 더미 노드(Dummy Node) 활용
엣지 케이스를 단순화하기 위해 실제 데이터를 갖지 않는 **더미 헤드/테일 노드**를 사용할 수 있다.

```python
class DoublyLinkedList:
  def __init__(self):
    self.head = DoublyNode(None)  # 더미 헤드
    self.head = DoublyNode(None)  # 더미 테일
    self.head.next = self.tail
    self.tail.prev = self.head
```


