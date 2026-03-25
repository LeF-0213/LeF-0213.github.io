---
layout: post
related_posts:
  - /frontend/python
title: "힙 정렬"
date: 2025-08-27
categories:
  - frontend
  - python
  - algorithm
description: >
---

- toc
  {:toc .large-only}

# Heap 정렬

1. 주어진 데이터로 합을 구축해야 한다.(정렬이 되어있지 않은 데이터로 힙을 구축)
2. 그 다음 힙정렬을 한다.

데어터의 삽입과 동시에 빠르게 정렬할 수 있다.

# Heap이란?

힙(Heap)은 **우선순위 큐**를 구현하기 위한 자료구조이다. 힙을 뜻을 살펴보면, "쌓아 올린 더미"라는 의미를 가진다. 힙은 **완전 이진 트리** 형태로 구성된다.

## 완전 이진 트리(Complete Binary Tree)란?

![CompleteBinaryTree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbfi34R%2FbtqV2WnM4Jj%2FAAAAAAAAAAAAAAAAAAAAALd34KSANB_SeFQH5a5Vyzp66U-iBwLiBStH0ga-UBtQ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1764514799%26allow_ip%3D%26allow_referer%3D%26signature%3D%252F263QoK6Pewpc%252BB30vAv0FKbuh8%253D)

#### 이진 트리(Binary Tree)

> 트리 구조에서 특정한 형태로 제한을 하게 되는데, **모든 노드의 최대 차수를 2로 제한 하는 것이다.** 즉, 각 노드는 자식노드를 최대 2개까지 밖에 못 갖는 것인데, 이를 **이진 트리(Binary Tree)**라고 한다.

#### 완전 이진 트리(Complete Binary Tree)

> **완전 이진 트리**는 **마지막 레벨**을 제외한 모든 노드가 체외져 있으면 모든 노드가 왼쪽부터 채워져 있어야한다.  
> 즉, 완전 이진 트리는 이진 트리에서 두가지 조건을 더 만족해야 한다.

> **규칙 1**: 노드를 왼쪽에서 오른쪽으로 하나씩 빠짐없이 채워나간다.(레벨 순서로 노드를 삽입한다.)  
> ![heap1](https://wikidocs.net/images/page/194445/ds-059.png)

- 위 그림 모두 이진트리인데, a는 힙이지만 b는 힙이 아니다.
- 왼쪽부터 채워야 한다는 규칙을 벗어났기 때문이다.

> **규칙 2**: 최소 힙은 부모 노드가 자식 노드의 값보다 작거나 같아야 한다. 파이썬의 heapq 모듈은 최소 힙(min heap)이다. (최대 힙은 부모 노드가 자식 노드의 값보다 크거나 같다.)

> 주의할 점: 부모와 자식간의 대소는 존재하지만, 자식간의 대소 규칙은 존재하지 않는다.

#### 포화 이진 트리(Perfect Binary Tree)

> 추가적으로 완전 이진 트리에서 마지막 레벨을 제외한 모든 노드는 모두 두개의 자식을 갖는다는 조건을 덧붙이면, **포화 이진 트리(Perfect Binary Tree)**가 된다.

> 즉, heap은 **최솟값** 또는 **최댓값**을 **빠르게** 찾아내기 위해 **완전이진트리** 형태로 만들어진 자료구조이다.

## Heap의 종류

![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlR7aO%2FbtqZSuVD2vb%2FAAAAAAAAAAAAAAAAAAAAAP4RP2moe9XAe3MSmP4fO98kxAr9SJZxiLWgCrSM97cU%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DuBAXwZEON8THrpfzJZDoaCH%252F%252F%252BA%253D)

### 최소 힙(Min Heap)

> **특징**: 부모 노드의 값(key 값) <= 자식 노드의 값(key 값)  
> **루트 노드**: 가장 작은 값
> **용도**: 최솟값을 빠르게 찾을 때 사용

### 최대 힙(Max Heap)

> **특징**: 부모 노드의 값(key 값) >= 자식 노드의 값(key 값)  
> **루트 노드**: 가장 큰 값
> **용도**: 최댓값을 빠르게 찾을 때 사용

## Heap을 배열로 구현하기

![MaxHeap&MinHeap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbaNi4n%2FbtqZ2csFHgz%2FAAAAAAAAAAAAAAAAAAAAABL6CYDeUa-Y5KmmgEwMQDXYxEwHLHG31Tpr_RgFltZ4%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DTGktBDy5FnF7TIcYVkyOIoOanH0%253D)

### 특징

> 구현의 용이함을 위해 시작 인덱스(root)는 1부터 시작한다.  
> 각 노드와 대응되는 인덱스는 '불변한다'

### 성질

> 부모 노드 인덱스 = 자식 노드 인덱스 / 2  
> 왼쪽 자식 노드 인덱스 = 부모 노드 인덱스 × 2  
> 오른쪽 자식 노드 인덱스 = 부모 노드 인덱스 × 2 + 1

## 힙의 삽입과 삭제 동작

### 삽입(Insert) 구현

#### heappush 구현하기(최소 힙)

> `heappush`는 이미 힙 구조를 가진 **배열의 끝에 새로운 원소를 추가**하는 함수이다. 이때 추가된 원소가 부모보다 작으면 최소 힙 구조를 유지하기 위해 부모와 자리를 바꾼다.

<div class="img-box" style="background-color: white; padding: 1rem; white; display: inline-block;">
  <img src="https://wikidocs.net/images/page/194546/ds-063.png" alt="insert">
</div>

> (a) -> 가장 끝에 2를 삽입한다.  
> (b) -> 2가 부모인 6보다 작으므로 서로 자리를 교환한다.  
> (c) -> 위로 올라간 2가 부모인 3보다 작으므로, 한번 더 교환한다..  
> (d) -> 힙의 성질을 만족했으므로, 끝낸다.

- **배열로 표현된 이진트리의 heappush 코드를 구현하기 위한 과정 정리**

> 배열(리스트) 끝에 새 값을 추가한다.  
> 추가한 원소의 인덱스를 구한다.  
> 부모 인덱스를 구하여 값을 비교한다.  
> 인덱스를 갱신한다. (자리를 바꿨으므로, 새로 추가한 값의 인덱스가 변함)  
> 같은 과정을 반복하며 루트에 도달하면 종료한다.  
> 혹은 새 값이 부모의 값보다 크거나 같으면 종료한다.

- **코드**

```python
# 부모 노드의 인덱스가 0일 때
def heappush(heap, data):
  heap.append(data)
  current = len(heap) - 1 # 추가한 원소의 인덱스를 구한다.
  # 현재 원소가 루트(인덱스 0)에 도달하면 종료
  while current > 0:
    # 추가한 원소의 부모 인덱스를 구한다.
    parent = (current - 1) // 2
    if heap[parent] > heap[current]:
      # 추가한 원소의 인덱스를 갱신한다.
      heap[parent], heap[current] = heap[current], heap[parent]
      current = parent
    else:
      break

# 테스트
h = [3, 4, 6, 8, 5, 7]
heappush(h, 2)
print(h)

# 결과
[2, 4, 3, 8, 5, 7, 6]
```

#### heappop 구현하기

> `heappop`은 힙에서 루트 노드(최솟값)를 꺼내고, 힙의 성질을 유지하도록 정렬을 다시 수행하는 함수이다.

<div class="img-box" style="background-color: white; padding: 1rem; white; display: inline-block;">
  <img src="https://wikidocs.net/images/page/194566/ds-062.png" alt="heappop">
</div>

> (a) -> 루트 노드의 값 3을 임시 저장한다.  
> (b) -> 마지막 노드인 7을 루트로 옮긴다.  
> (c) -> 루트 노드 7을 자식 노드인 4와 6 중에서 작은 값인 4와 자리를 바꾼다.  
> (d) -> 다시 7을 자식 노드인 8과 5 중에서 작은 값인 5와 자리를 바꾼다.

**왜 마지막 노드를 꺼내 루트로 옮기는 걸까?**

> `heapq.heappop(heap)`은 최소힙(min-heap) 구조에서 루트 노드(가장 작은 값)을 꺼내는 연산이다.  
> 그런데 힙은 **완전 이진 트리** 형태를 유지해야 하기 때문에,  
> 마지막 레벨을 제외하고는 꽉 차 있어야 한다.  
> 때문에 트리의 가장 끝에 있는 **마지막 노드는 제거해도 트리의 완전성이 유지된다.**  
> 즉, 맨 끝 노드를 루트로 옮기면, 구**조는 그대로 두고 값만 재배열**하면 된다.

- **코드**

```python
def heappop(heap):
  # 노드가 하나라도 있으면 pop()
  if not heap:
    return None
  elif len(heap) == 1:
    return heap.pop()

  # 루트 노드의 값을 임시저장하고, 마지막 노드를 루트로 이동
  pop_data, heap[0] = heap[0], heap.pop()
  # 현재 노드와 자식 노드의 인덱스로 0, 1을 대입한다.
  current, child = 0, 1
  # 자식 노드의 인덱스가 heap의 길이보다 작으면 반복
  while child < len(heap):
    sibling = child + 1
    if sibling < len(heap) and heap[child] > heap[sibling]:
      child = sibling
    if heap[current] > heap[child]:
      heap[current], heap[child] = heap[child], heap[current]
      current = child
      child = current * 2 + 1
    else:
      break
  return pop_data

# 테스트
h = [3, 4, 6, 8, 5, 7]
data = heappop(h)
print(data)
print(h)

# 결과
7
[3, 4, 6, 8, 5]
```

## 최소 힙 클래스 구현

```python
class MinHeap:
  def __init__(self):
    self.heap = []

  # 부모 노드의 인덱스 반환
  def parent(self, i):
    return (i - 1) // 2

  # 왼쪽 자식 노드의 인덱스 반환
  def left_child(self, i):
    return 2 * i + 1

  # 오른쪽 자식 노드의 인덱스 반환
  def right_child(self, i):
    return 2 * i + 2

  # 두 노드의 위치 교환
  def swap(self, i, j):
    self.heap[i], self.heap[j] = self.heap[j], self.heap[i]

  # 새로운 값을 힙에 삽입
  def insert(self, value):
    self.heap.append(value)
    self._heapify_up(len(self.heap) - 1)

  # 삽입된 노드를 적절한 위치로 이동 (상향식)
  def _heapify_up(self, i):
    parent = self.parent(i)

    if i > 0 and self.heap[i] < self.heap[parent]:
      self.swap(i, parent)
      self._heapify_up(parent)

  # 최솟갑(루트) 제거 및 반환
  def extract_min(self):
    if not self.heap:
      return None

    if len(self.heap) == 1:
      return self.heap.pop()

    # 루트를 저장하고, 마지막 노드를 루트로 이동
    min_value = self.heap[0]
    self.heap[0] = self.heap.pop()
    self._heapify_down(0)

    return min_value

  # 루트 노드를 적절한 위치로 이동 (하향식)
  def _heapify_down(self, i):
    min_index = i
    lef = self.left_child(i)
    right = self.right_child(i)

    if left < len(self.heap) and self.heap[left] < self.heap[min_index]:
      min_index = left

    if right < len(self.heap) and self.heap[right] < self.heap[min_index]:
      min_index = right

    if min_index != i:
      self.swap(i, min_index)
      self._heapify_down(min_index)
      self._heapify_down(min_index)

  # 최솟값 조회 (제거하지 않음)
  def peek(self):
    return self.heap[0] if self.heap else None

  # 힙의 크기 변환
  def size(self):
    return len(self.heap)

  # 힙이 비어있는지 확인
  def is_empty(self):
    return len(self.heap) == 0
```

## 최대 힙 클래스 구현

```python
class MaxHeap:
  def __init__(self):
    self.heap = []

  def parent(self, i):
    return (i - 1) // 2

  def left_child(self, i):
    return 2 * i + 1

  def right_child(self, i):
    return 2 * i + 2

  def swap(self, i, j):
    self.heap[i], self.heap[j] = self.heap[j], self.heap[i]

  def insert(self, value):
    self.heap.append(value)
    self._heapify_up(len(self.heap) - 1)

  def _heapify_up(self, i):
    parent = self.parent(i)

    # 최대 힙: 부모보다 크면 교환
    if i > 0 and self.heap[i] > self.heap[parent]:
      self.swap(i, parent)
      self._heapify_up(parent)

  def extract_max(self):
    if not self.heap:
      return None

    if len(self.heap) == 1:
      return self.heap.pop()

    max_value = self.heap[0]
    self.heap[0] = self.heap.pop()
    self._heapify_down(0)

    return max_value

  def _heapify_down(self, i):
    max_index = 1
    left = self.left_child(i)
    right = self.right_child(i)

  # 최대 힙: 자식 중 더 큰 값과 비교
  if left < len(self.heap) and self.heap[left] > self.heap[max_index]:
    max_index = left

  if right < len(self.heap) and self.heap[right] > self.heap[max_index]:
    max_index = right;

  if max_index != i:
    self.swap(i, max_index)
    self._heapify_down(max_index)

  def peek(self):
    return self.heap[0] if self.heap else None
```
